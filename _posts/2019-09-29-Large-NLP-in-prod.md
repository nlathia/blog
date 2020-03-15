---
layout: post
title: When is a neural net too big for production?
description: Here are some thoughts on the recent discussions around NLP transformer models being too big to put into production, and a dive into how we have shipped them at Monzo using the HuggingFace library.
categories: [opinion, monzo]
---

### 💬 Background
Over the last couple of years, there have been a ton of exciting developments in natural language processing (NLP). In case you haven't been working in this area, here's the crash course: the development of [deep pre-trained language models](http://ruder.io/nlp-imagenet/) has taken the field by storm. In particular, [transformer architectures](https://arxiv.org/abs/1706.03762) are everywhere, and were popularised by Google's release of the [**B**idirectional **E**ncoder **R**epresentations from **T**ransformers](https://arxiv.org/abs/1810.04805) (BERT) model, OpenAI's release of the [GPT(-2) models](https://openai.com/blog/better-language-models/), and other similar releases.

Various research teams are continuing to compete to train better language models; if you look at the [General Language Understanding Evaluation (GLUE)](https://gluebenchmark.com/leaderboard) benchmark leaderboard, you'll find a host of other approaches (many of them also named BERT: ALBERT, RoBERTa, etc.). The overarching trend in this research has been to train **bigger** models with **more data** - growing to the extent that researchers [have investigated](https://arxiv.org/abs/1906.02243) the costly carbon footprint of training these large networks.

For practicioners, the main selling point of pre-trained language models is that you do not need to start from scratch when developing a new text classifier: you can fine tune a pre-trained model and often get state-of-the-art results with a fraction of the work. But as these models continue to grow in size, folks have started to question how useful they are in practice. For example, this quote in [a blog post](https://explosion.ai/blog/spacy-pytorch-transformers) from the explosion.ai team caught my eye (emphasis mine):

> In a recent talk at Google Berlin, Jacob Devlin described how Google are using his BERT architectures internally. **The models are too large to serve in production, but they can be used to supervise a smaller production model.** 

Unfortunately, it looks like this talk [was not recorded](https://twitter.com/honnibal/status/1157592067712966657), and so all of the context around this claim was lost. In light of that, this post gives an overview of how these models _can_ and _have been_ put into production.

### 🚢 Patterns for models in production
Let's begin with a very pragmatic question: **what is "production?"**

For our purposes, production is the environment where we put software that we have written (and models we have trained) so that it can enable features in a product to work without any manual intervention. By this definition, we exclude any code that we use for analytics or ad-hoc purposes, even though there may be potential applications of NLP in those domains (e.g., sentiment analysis on historical data).

I'm also assuming that this environment is designed using [microservices](https://en.wikipedia.org/wiki/Microservices) -- which just so happens to be how the [Monzo backend is designed](https://monzo.com/blog/2016/09/19/building-a-modern-bank-backend).

There are three main ways that models can be used in production:

**RESTful Services**. This is the first (and sometimes only thing) that comes to mind when folks talk about "production." The idea is to build some kind of microservice with an API which can receive requests, do some work (i.e., get predictions from a model), and return a result. For example, when a customer types a query into the Monzo app's help screen, we have a service that receives that request and returns relevant help articles (disclaimer: that has been simplified a bit. We have quite a few services that are involved in this work, but the idea is the same).

**Consumer Services**. The second approach is to build a service which listens out for certain events and requests for some work to be done when they happen. For example, when a customer starts chatting with our customer support team, we have a service that is listening for particular events in order to (a) generate embeddings of the chat's first turn, and (b) trigger recommendations that are shown to the agent for saved responses that may be relevant to the current query.

**Cron Jobs**. These are batches of work that need to be done on a regular basis. For example, we store all of the help articles and agent responses in a content management system - and these are regularly edited and updated with new content. Our search and recommendation services use the _embeddings_ of this content: we therefore have cron jobs that regularly run, encode all of the text and store the embeddings so that they can be used when needed.

In practice, building an end-to-end system is likely to involve more than one of the above. I've already mentioned the system which gives our agents response recommendations: this system has a cron job (to encode all of the response text), a consumer service (which decides when recommendations should be triggered), and a RESTful service (which is, effectively, a k-Nearest Neighbour between the encoded customer text and the encoded responses).  

### 🙅‍♂️ When is a model too big?
Now that I've described three generic ways that models are shipped, let's tackle the main question: when is a model too big? There are two scenarios to consider: (1) a model is too big _to ship at all_, and (2) a model's size is making it inefficient.

**Too big to ship at all?** The main question that may prevent shipping a model at all is about reconciling the hardware (where you want to run a model) with the size of the model. In practice, current models' sizes are not a big problem in cloud-based backend systems, which have a variety of different instance sizes available - the hardware we have in the cloud _can_ ship a model like BERT. It may eat up a ton of memory - but it will _work_. 

This could change if you want to want to ship a model elsewhere (for any other reason). For example, [this ICML workshop talk](https://slideslive.com/38917690/multitask-learning-in-the-wilderness) by Andrej Karpathy describes a large multi-task neural net that needs to run in a car, or folks at Google are investigating [federated learning](https://ai.googleblog.com/2017/04/federated-learning-collaborative.html) in order to distribute model training across mobile phones. This is part of a broader movement that is pushing machine learning towards "edge" devices, which are generally resource and energy constrained.

**Too big to be efficient?** Models are often trained using GPUs, but shipped on non-GPU instances, where inference will be slower. As models get bigger, inference time often continues to grow. There may be a point where this slow down makes it infeasible. This is going to be a very application-specific decision: for example, a chat bot responding within a few seconds may still be "fast" in the customers' eyes, while if it took a similar time to get search results on Google, something would seem odd. To dig deeper, let's reference the three patterns above.

In cron job settings, inference time is usually not such a big deal - predictions can be batched, and need to be completed on a schedule. Performance will become more of an issue as the amount of data grows, and we can then consider parallelising the problem to make it faster.

In the other two patterns, things become even more application specific. Consider, for example, the system I mentioned above that is consuming chat events to decide to push saved response recommendations to our agents. In this case, the time the system needs to generate those recommendations should be (broadly) less than the time that it takes an agent to read through what has been written so far - this use case is measured in the order of multiple seconds; i.e., nearly an eternity for computers.

Finally, we have services that we are experimenting with to try and [improve the app help screen](https://monzo.com/blog/2018/08/01/data-help) - some of these are using BERT. In our first experiment, we saw that some of these services were struggling under the load they were receiving - but the first port of call is to [scale them horizontally](https://github.com/vaquarkhan/vaquarkhan/wiki/Difference-between-scaling-horizontally-and-vertically) rather than pull the handbrake and not deploy them at all. This means that we are trading off between how many instances we need (or want) to spin up and the performance we want to achieve, much like what happens when these same models are trained on large clusters.

### 🤗 Example: serving BERT predictions
At Monzo, we have decided for our Python microservices to be as lightweight as possible: they are effectively a nice wrapper around a model's `predict()` function, and we write the rest in [Go](https://golang.org/) - the main language that is used throughout the Monzo backend.

We have built a [cookiecutter](https://cookiecutter.readthedocs.io/en/latest/) Python microservice template that uses [Sanic](https://github.com/huge-success/sanic). When one of these services is starting up, it needs to do two, fairly slow, things: (1) find and retrieve the model that it wants to serve (I'll blog about that problem separately), and (2) load the model (in `eval()` mode) and the tokenizer as `global` variables in memory. 

(Side note: I originally intended to share some code snippets here, but this jekyll theme stubbornly refused to be mobile friendly. So I've removed it - but it's not far off from the available examples online.)

I did a small test on my own laptop using [this approach](https://stackoverflow.com/questions/938733/total-memory-used-by-python-process) which uses `psutil` to measure the "Resident Set Size" memory usage (is this the right way? 🤷‍♂️). Before loading the model, memory usage was about 79 MB: after the call to `load_model()`, it shot up to just over 957. A huge jump, yes (and 100s of times bigger than what you would expect in non-machine learning services) - but still well below what decent cloud instances provide. 

Once these steps have finished, the service will start serving requests. Each of these services will have an endpoint (or [Sanic route](https://sanic.readthedocs.io/en/latest/sanic/routing.html)) to get the model's predictions for a given input.

The `async` and `await` syntax in Sanic routes is the key here: handlers are an [async co-routine](https://docs.python.org/3/library/asyncio-task.html), and all model predictions are run in the `asyncio` event loop, rather than as blocking functions. There are many blog posts that describe the details of the Python event loop and the `async / await` syntax much better than I ever could; if you're interested, I'd recommend searching for this topic ([here's one](https://stackabuse.com/python-async-await-tutorial/)).

The `model_predict()` runs model predictions with `torch.no_grad()`: this ensures that the autograd engine is not used. According to [this thread](https://discuss.pytorch.org/t/model-eval-vs-with-torch-no-grad/19615), this reduces memory usage and speeds up computation. Naturally, you can't then backpropagate (needing to do so while serving is an entirely different type of problem!).

There was one tiny trick that two folks on the team discovered which helped us to make these types of services _even faster_. They discovered the issue that it seems [others have also found](https://twitter.com/MarkNeumannnn/status/1067926695338926080) regarding threading performance and the `OMP_NUM_THREADS` and `MKL_NUM_THREADS` environment variables; the one difference was that they also had to factor in how all of this plays with Sanic [worker threads](https://sanic.readthedocs.io/en/latest/sanic/deploying.html#workers).

### 🔍 Reflections
The main disclaimer that I'll add to the above is that we are currently in the stage of development where we're iterating on and validating new product features, and not squeezing performance gains out of existing ones. Perhaps, once we reach that stage (where every megabyte of memory, instance we spin up, and microsecond matters), I'll change my mind about BERT being suitable for production 😊. 

So, after writing an entire blog post about how we can (and do) but large models like BERT into production, I'll close with two thoughts.

First, the research on [distilling](https://medium.com/huggingface/distilbert-8cf3380435b5) and [compressing](https://arxiv.org/abs/1909.11687) these models into smaller ones that retain similar levels of accuracy is still very valuable. It will unlock our ability to (a) run these models more efficiently, and (b) run them on edge devices at all. Just as training larger models with more data is showing impressive feats of engineering and distributed model training, doing more with less seems to push researchers towards hard, unsolved problems of understanding how neural nets learn at all.

Second: the main (hidden) assumption that I've had throughout this whole post is that an **entire** neural net should be shipped as a **single** service. Maybe this doesn't have to be the case: a network could be broken up into multiple microservices (e.g., imagine freezing an entire pre-trained network and then shipping multiple different fine-tuned heads into different services). I haven't been able to find a lot of blog posts about machine learning in production - if you find (or write!) one, do [send it my way](https://twitter.com/neal_lathia).

Acknowledgements: thanks to [Stephen Whitworth](https://stephen.sh/) for feedback on a draft of this post.
