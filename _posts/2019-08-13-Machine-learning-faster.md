---
layout: post
title: Machine learning, faster
description: I recently gave a couple of conference presentations about how we are thinking about _speed_ when developing machine learning systems at Monzo. This post covers some of the background to the points I was making in my talks, as well as what we're doing in the Monzo machine learning team to speed up our own work.
categories: [opinion, monzo]
---

Speed is not a word that is regularly associated with machine learning teams. When we talk and write about accomplishments in machine learning, there is often a focus on the problem, the algorithmic approach, and the results - but no mention of the time that it took to get there.

### Does speed matter in machine learning?
I remember once speaking with a machine learning researcher who worked at a large company. He told me that a product team had approached him with a very exciting idea that had to do with text summarisation. He started looking into the problem and made some very significant contributions over the course of 12 months - going so far as publishing papers in top-tier conferences about the topic. I asked him if his ideas made it into the product in question. Unfortunately, the answer was no: by the time his research was completed, the product team had moved on from this problem, and weren't interested in having the solution anymore 😭.

I've heard many variants of this story: they all capture a misaligned pace of work between product and machine learning teams. Ultimately, this leads to machine learning research never making it out of the lab. And yet, the best measure of _impact_ for machine learning, if you work in a non-research institution, is whether you can use it to help your customers - and that means getting it out of the door.

Speed matters. This goes beyond thinking about minimum viable products (and the ML equivalent of "use a logistic regression before you use a neural network"); this is about the speed of the entire lifecycle for building machine learning systems.

I covered four angles to this topic, when I gave talks about it recently:

### 🚢 Quickly deploying models to production
Quickly deploying models to production is one of the biggest roadblocks for impactful machine learning. In many companies, this boils down to _who is trusted_ to do this work; often, 'Scientists' design and train a model, and then hand it over to 'Engineers' to put it into production. This implicitly develops a "throw it over the wall" mentality: people who train models do not have to think about how complex it would be to ship, and folks who ship models can throw a model back over the wall if it's not behaving how it should. The most common complaint I've had from _Scientists_ is along the lines of "I trained this model months ago and I'm just sitting here waiting for it to be shipped," and _Engineers_ retort that they get no recognition for doing all of the hard work of enabling production inference to actually happen. A frustrating experience all around.

At Monzo, Machine Learning Scientists deploy their own models. We are enabling that by focusing on our tooling. The goal that we set ourselves is that if you can write a `predict()` function for a machine learning model, you should also be able to **safely** deploy a model to production. To enable this, we've built a `cookiecutter` microservice template that Machine Learning Scientists use: it guides them through a small set of options (e.g., "which ML library are you using?") and then creates everything they need, minus small bits, like the `predict()` function itself. We can now build and deploy a service in a matter of hours - right after we finish training a model. A _Scientist_ does not have to wait around for others to finish the work, and can take an idea all the way from validation to deployment.

### 🔍 Quickly validating misbehaviours
Last year, after a re-launch of our [help screen search system](https://monzo.com/blog/2017/08/22/the-help-search-algorithm), a very common question that we would get from product teams is "when I search for `X`, why doesn't article `Y` show up?" Trying to explain machine learning algorithms is hard enough - diagnosing minor misbehaviours felt even more challenging. Has something gone wrong with our data pipeline, our model, or how we are post-processing the results down stream? 

This is a type of problem that could grind us to a halt: it's not the sort of thing we could write unit (or integration) tests for, and neither is it something where the problem is immediately clear. To tackle this quickly, we started writing **validation tests**: these are a set of fairly simple inputs (e.g. "moving home") that have obvious outputs (the [updating my home address](https://monzo.com/help/account-and-profile/update-home-address) article), which we expect our **production** machine learning system to be able to return when making predictions. These run in production: they are integrated into how we update the embeddings that are stored in the system, and prevent updates from happening if expected predictions fail (also, we get notified on Slack ⚠️ ). 

Keeping track of the online performance of machine learning models is going beyond what we traditionally do when deploying software - writing validation tests is another small improvement that has helped us to keep on top of how these systems operate online.

### ♻️ Quickly repurposing models for new problems
Last year, Monzo went through a [challenging period](https://monzo.com/blog/2018/12/17/customer-support) where non-urgent response times for customer support was in the order of days rather than hours. The entire company rallied behind this: engineers, designers, and lawyers all dropped what they were doing to respond to customers.

As part of enabling the entire company to answer customers, a set of topic-based queues were set up; the idea was that it is easier to train someone to answer one type of question rather than answer any kind of question. Our team was asked whether we could set up a way to detect what customers were talking about and automatically assign queries to queues accordingly. Typically, machine learning teams would start here with the usual questions (e.g., what data do we have?) and methods (e.g., training a classifier). However, this wasn't a request to kick-start a project looking into text classification: this was a request to get something working and deployed, now.

One of the systems that we were already running at the time served recommendations to customer support agents - it looked at what customers said and returned recommendations for which of our many different saved responses may be appropriate to use. For example, if a customer says "I've forgotten my PIN," the system recommends a response that contains all of the details of how to recover it. This system is not explicitly _categorising_ queries; it is mapping questions to recommended answers based on a similarity metric. However, the recommended answers could easily be mapped to categories!

We therefore quickly repurposed this system's recommendations to solve for the queue classification problem: we wrapped the recommendations in `if` statements to redirect queries to queues based on what reply was recommended. This was deployed this in less than a day!

This lesson has stuck with us since then: combining rule engines _with_ machine learning models can be used to quickly create, contextualise, or repurpose a decision system.

### ⏰ Measuring time-to-results, not results
The systems mentioned above use an encoder architecture that was published in 2017 (based on the _Attention is all you need_ [paper](https://arxiv.org/abs/1706.03762)). While working on it, we felt that, overall, we were taking advantage of latest research when building our systems. Then, in 2018, deep pre-trained language models (like ELMo, ULMFit, BERT) appeared and started to take the top spot in a variety of research challenges - and many of them were open sourced. Every few months the state-of-the-art was changing. As a non-research team, that focuses on building systems, how could we keep up with this pace of research? 

We decided to set ourselves a challenge: to explore how these techniques could work on as many different NLP problems as we could find, in a really short time (a couple of weeks). A typical approach here, which I have used before, is to embark on a new project for each idea: analyse the data, understand/refine the problem, train and evaluate models, and tweak until something starts looking promising. There was no way that, using this approach, we could evaluate tens of ideas in a fortnight. Instead, rather than trying to answer the question "what is the best result I can get?," we designed our work around "how long will it take to get a result?"

Switching our perspective led us towards taking a highly modular approach to our NLP work. We built a pipeline that creates supervised learning datasets from all of our chat data: we just point it to what we want to predict, and give it some additional arguments to tweak how we want the text to be processed. This tool is completely separate from anything that does any machine learning; for that, we built a number of Colab notebook **templates**, which take (as input) a dataset, and then run it through a specific algorithm (e.g., ULMFit). Our guiding principle was that anyone should be able to point a notebook at their dataset, and then run all the cells to get to a result.

Many of the experiments that we ran had poor results - and we have thrown them away. However, the pipelines that we used to get them have stayed and evolved, and are regularly used to generate the data that trains all of our models. This experience changed my perspective on _research_ time: it is time well spent if we learn about new things and develop _tools_ that allow us to apply it quickly (if we also get some good results, that's a bonus!).


### ⬇️ Conclusions
There's a famous quote that I've often heard: "to increase your success rate, double your failure rate" (it looks like [Thomas Watson](https://en.wikiquote.org/wiki/Thomas_J._Watson) said this). This is as true in machine learning as it is anywhere else.

In the machine learning team at Monzo, we've been thinking about this in terms of the _speed_ of our work. This is not about cutting corners, being less rigorous, or skipping important steps of the work. Speed is about setting ourselves up to get results quickly, repurposing systems quickly, validate online behaviour quickly, and deploy to production quickly. 

This post covers a few stories that shaped our views on speed - but this is always a work in progress. More posts to come! It is based on these two talks:

* Speeding up Machine Learning Development April 2019, Xcede Data Science Networking Event. London. 
* Using Deep Learning to Support Customer Operations March 2019, ReWork Deep Learning in Finance Summit. London.

You can find the slides from my talk [here](https://www.slideshare.net/neal.lathia/machine-learning-faster); reach out [on twitter](https://twitter.com/neal_lathia) if you have any thoughts!