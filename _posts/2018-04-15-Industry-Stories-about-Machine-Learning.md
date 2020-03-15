---
layout: post
title: Industry stories about machine learning
description: My notes from the PAPIs.io 2018 conference in London.
categories: [conference]
---

Last week, I went to the [PAPIs.io Europe 2018](https://www.papis.io/europe-2018) conference, which was held in Canary Wharf Tower in London. The conference describes itself as a “series of international conferences dedicated to real-world Machine Learning applications, and the innovations, techniques and tools that power them” (and, from what I gather, the name _papis_ comes from “Predictive APIs”). I went down on the Thursday, the day that was dedicated to “Industry and Startups,” and took some notes on what I saw. Here’s a quick summary!

<img data-width="1296" data-height="592" src="https://cdn-images-1.medium.com/max/1200/1*7pig2ezHCXRvZNsshPYpgw.png">

## ML infrastructure with Kubernetes, Dask, and Jupyter
The morning keynote was by [Olivier Grisel](http://ogrisel.com/), who is probably best known for his immense contributions to [scikit-learn](http://scikit-learn.org/stable/) — and therefore anyone who does machine learning in Python is indebted to him! His slides are online [here](https://docs.google.com/presentation/d/1PuvBl2rqZi0k0J2x4XcShmFiLeSfBXkarg6nYAGCxUs/edit#slide=id.p)

In [this video](https://www.youtube.com/watch?v=9zEAC89QcjA), starting around 23 minutes in, he shows how to set up your own machine learning infrastructure using three main open source components: [Kubernetes](https://kubernetes.io/) (a cluster orchestration system based on [containers](https://en.wikipedia.org/wiki/Operating-system-level_virtualization)), [Dask](https://dask.pydata.org/en/latest/) (a tool to parallelize python jobs that is deeply integrated with existing python libraries like pandas/numpy), and [Jupyter Notebooks](http://jupyter.org/) (the well-known web application for interactive development).

Specifically, he was using [minikube](https://kubernetes.io/docs/getting-started-guides/minikube/) to run kubernetes locally and [jupyter hub](https://jupyterhub.readthedocs.io/en/latest/) to manage multiple users on a single jupyter server. The first example that he showed was somewhat trivial (e.g., incrementing a counter), but this allowed him to describe, in depth, how the computation was being distributed and executed. The second example showed how to run a [grid search](http://scikit-learn.org/stable/modules/grid_search.html#exhaustive-grid-search) to find the best parameters for a Support Vector Machine, using [dask-joblib](http://distributed.readthedocs.io/en/latest/joblib.html) to run this on the cluster.

One of my favourite lines from the Q&amp;A was an off-hand comment that touches on developing ML systems: “you shouldn’t do everything in a Jupyter notebook” (because that’s not great for maintenance).

## Prediction at the edge with AWS
The second talk was by [Julien Simon](https://aws.amazon.com/evangelists/julien-simon/) (who blogs [here](https://medium.com/@julsimon)), an AI/ML evangelist from Amazon. Starting at about minute 59 in [this video](https://www.youtube.com/watch?v=9zEAC89QcjA), his talk focused on running machine learning predictions outside of data centers (‘at the edge’ — on cameras, on sensors, etc.). Achieving this entailed, perhaps unsurprisingly, going through a whirlwind tour of various AWS services that are available for machine learning systems. These included:
* Defining and manipulating models with [Gluon](https://aws.amazon.com/blogs/aws/introducing-gluon-a-new-library-for-machine-learning-from-aws-and-microsoft/) and [MXNet](https://aws.amazon.com/mxnet/);
* Building and training models with [SageMaker](https://aws.amazon.com/sagemaker/);
* Using [Lambda](https://aws.amazon.com/lambda/) to write on-demand prediction functions;
* Deploying the code to edge devices using [Greengrass](https://aws.amazon.com/greengrass/).

His talk closed with a demo of [DeepLens](https://aws.amazon.com/deeplens/), the “world’s first deep learning enabled video camera for developers” which was recently launched, showing real-time object detection in action.

## Managing the gap between the engineer and data scientist role
One of the talks that touched on very interesting topics was by [Beth Logan](https://www.linkedin.com/in/bethlogan) from [dataxu](https://www.dataxu.com/), a data-driven advertising company. She described how they go about developing and automating the deployment of a machine learning pipeline (hence the title ‘changing tires while driving;’ the talk is online [here](https://www.youtube.com/watch?v=WV_o7B4YcNw)) to support various applications in the advertising domain.

Moving away from the ML itself, there were some interesting points made about how to manage what a ‘data scientist’ does vs. what an ‘engineer’ does, in order for each role to play to their strengths. In effect, this was about letting data scientists develop and iterate on models, while leaving all of the job of productionising and scaling them to engineers — who also had to demonstrate that the production implementation performed as expected.

The intersection of data science and engineering is a topic that I could probably write an entire blog post about; suffice to say, we had a discussion at the end about whether such a divide is the ‘right’ way to do this, and how each discipline can upskill the other while collaborating.

## Pipeline jungles in machine learning
The next talk was by [Moussa Taifi](https://sites.google.com/site/moussataifishomepage/) from [Appnexus](https://www.appnexus.com/en), another company that deals with digital advertising. He discussed building various kinds of pipelines for click prediction, a common task in online advertising.

Moussa touched on a number of practical aspects of developing pipelines while going back and forth between research and production. These included getting into trouble with reproducing results once pipelines are overly complex (‘jungles’), model versioning for experiments, avoiding common issues like time travel (training on data that was created after the data in the test set), and whether it is better to go for systems with just-in-time data transformations and feature extraction vs. building models from a fixed set of features that are precomputed, regardless of the task at hand

## Building a culture of machine learning
[Lars Trieloff](https://www.linkedin.com/in/trieloff/) gave a high-level talk about nurturing a culture of AI inside of [Adobe](https://www.adobe.com/uk/#) — focusing specifically on [Adobe Sensei](https://www.adobe.com/uk/sensei.html). His talk spanned three broad areas: _brand, vision, and technology_, and how the three needed to gel in order to foster a culture of machine learning within an organisation. Interestingly, he also touched on responsibility — and how all employees at the company needed to go through training and approval process when developing new machine learning tools.

## Feasibility vs return on investment in machine learning
Poul Petersen from [BigML](https://bigml.com/) gave a talk about how the company predicted 6 out of 6 of the 2018 Oscar winners — see [this blog post](https://blog.bigml.com/2018/03/01/predicting-the-2018-oscar-winners/) which has some similar content. Oscars aside, he made an interesting observation about how to prioritise machine learning projects, based on comparing their feasibility and projected return on investment. If both are low, this is clearly a no-go area; if both are high, this is a no-brainer that you should already be working on. The remaining two categories were ‘postponable’ (low ROI, highly feasible) and ‘brainers’ (high ROI, not currently feasible).

He gave a similar analogy for the progression of which algorithms were his go-tos, depending on what stage of development a particular system was at: early stage, requiring rapid prototyping (logistic regression), mid stage, where you have a proven application (random forests), and finally late stage, where tweaking performance becomes critical (neural networks).

## Startup pitches & panel — the European AI landscape
The startup pitches were dispersed across the day. The ones I saw here:
* [Logical Clocks](http://www.logicalclocks.com): who have an enterprise machine learning platform called [Hops](http://www.logicalclocks.com/deep-learning/) that aims to improve the productivity of data scientists.
* [Antiverse](http://www.antiverse.io/): aim to enable antibody drug discovery in one day using AI.
* [Tensorflight](https://tensorflight.io/): automate property inspections by analysing satellite and aerial data using machine learning.
* [Teebly](https://teebly.co/): offers a single point of contact for a business’ clients, automating all of the various ways that they can get in touch with you.

Some of these startups participated in a startup battle at the end of the day, which was [judged by an AI](https://medium.com/@thomasrorystone/1st-startup-competition-judged-by-an-ai-apply-to-win-100-000-from-ai-seed-7ce5cb42df72). While I was somewhat sceptical when I first heard about this, it was actually very entertaining. Each startup took turns being asked questions by an Alexa, with questions ranging from the size, experience, and structure of the team, and were scored on a variety of factors. The winners took home £100k!

The startup panel, instead, took a retrospective perspective — looking back at what worked for Twizoo (which was [acquired by Skyscanner](https://techcrunch.com/2017/11/09/skyscanner-buys-twizoo-to-add-social-content-shout-outs-to-travel-reviews/), shortly before I left), [prediction.io](http://predictionio.apache.org/index.html) (which was [acquired by Salesforce](https://techcrunch.com/2016/02/19/salesforce-acquires-predictionio-to-build-up-its-machine-learning-muscle/) after being founded in London), and [Seedcamp](https://seedcamp.com/). The recurring theme was the importance of focusing on the _customer_, rather than the machine learning: technology is an enabler to solve a customer’s pain, and the abstract machine learning problems that need to be solved along the way are nearly superfluous compared to the customer’s need.

There are many different take-aways from this day. One that stands out is that the European [startup landscape in the machine learning](https://medium.com/cityai/the-european-artificial-intelligence-landscape-more-than-400-ai-companies-build-in-europe-bd17a3d499b) space is still thriving and growing. And, indeed, [Libby](https://twitter.com/libbykinsey) from [Project Juno AI](https://medium.com/project-juno) announced that they are starting another round of mapping this landscape — a project that’s definitely worth checking out and contributing to.