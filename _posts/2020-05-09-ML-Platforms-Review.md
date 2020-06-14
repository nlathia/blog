---
layout: post
title: The panacea of machine learning platforms
description: 
categories: [machine-learning]
---

The words "machine learning platform" sound like a panacea.

Notes:

[TFX](https://www.kdd.org/kdd2017/papers/view/tfx-a-tensorflow-based-production-scale-machine-learning-platform)

[TFX: A TensorFlow-Based Production-Scale Machine Learning Platform](https://dl.acm.org/doi/pdf/10.1145/3097983.3098021)

> Unfortunately, such orchestration is often done ad hoc using glue code and custom scripts developed by individual teams for specific use cases,leading to duplicated effort and fragile systems with high technical debt

> reduce the time to production from the order of months to weeks

Core concepts: DAGs
> Most machine learning pipelines areset up as workflows or dependency graphs that execute specific operations or jobs in a defined sequence

Training phase problems:
- at the training  phase, the learner takes a dataset as input and emits a learned model
- continuous training  over  evolving  data
- tracking experiment history; switching out the learning algorithm with no infra change
- data sampling, feature generation
- analysis, visualisation (e.g., TF board)
- Bugs in the upstream data logging
- Experimentation: single run, optimisation run (time to results, distributed computation/infra)

Inference phase:
- at the inference phase, the model takes features as input and emits predictions
- to deal with a diverse range of failures that can happen in production and to ensure that model training and serving happen reliably
- deploy and monitor the platform with minimal configuration
- Validation/safe to deploy/shadow mode
- Multitenancy (2 models in same service; memory)
- Inputs from other ML models
- Latency

Coupled across phases:
- Coupling between jobs; matching between training & production data
> model validation must be coupled with data validation in order to detect corrupted training data
- Applying transformations on the data
- Lookups (e.g. encodings, embeddings)
- Versioning of software
- Human-in-the-loop for data annotation

Missing:
- "Maturity" of a piece of code/data feature (experimental nature of data)

[FBLearner Flow](https://engineering.fb.com/ml-applications/introducing-fblearner-flow-facebook-s-ai-backbone/) - 2016

Problems:
- Speed of experimentation
- Same pipeline different inputs (parameter sweeps)
- Result visualisation
- Algorithm re-use (single implementation)
- Training parallelisation
- Automate "every step" and enable those who are less familiar with ML
- Sharing & discovering experiments

Approach:
- Defining an entirely new abstraction (workflow, operators, channels)


Tecton.ai is [solving too much](http://roundup.fishtownanalytics.com/issues/google-tapas-the-data-science-job-market-tecton-data-science-lyft-pinot-supports-sql-dsr-225-247259)

http://featurestore.org/

https://insights-dice-com.cdn.ampproject.org/c/s/insights.dice.com/2020/05/04/machine-learning-engineer-challenges-changes-facing-profession