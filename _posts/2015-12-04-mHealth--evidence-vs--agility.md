---
layout: post
title: "mHealth: evidence vs. agility"
categories: [opinion, research]
---

While mHealth apps are [flooding the market](http://mhealtheconomics.com/largest-global-study-on-mhealth-released-there-are-more-than-165000-mhealth-apps-today/), the majority of apps that are currently available [lack scientific evidence about their efficacy](http://www.jmir.org/2013/11/e247/). I have found that this point isn’t debated as much as it should: in research settings, it is taken for granted _as a problem_.

Why should this be debated more? I’ve created two perspectives:

### Group 1: Pro-Evidence
The pro-evidence camp are asking a number of very important questions. Do health apps really change behaviour? Are they safe, evidence-based tools that do what they claim? Arguments here always draw comparisons between software and medicine:

> “We need to do high-quality clinical studies and apply the same standards that you would to a new pharmaceutical.” (Torous, [Scientific American](http://www.scientificamerican.com/article/should-you-take-an-app-for-that/))

This seems very sensible. Particularly if we want to reach a world where clinicians can [prescribe software](http://www.wsj.com/articles/doctors-prescribe-new-apps-to-manage-medical-conditions-1447094444): they need to know that it works.

Instead, it seems that health apps have gone the other direction. A study of the content of [47 smoking cessation apps](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3395318/) shows an inverse relation between app popularity and behavioural evidence; there are similar papers for [chronic pain management](http://www.ncbi.nlm.nih.gov/pubmed/21844177) apps; for [melanoma diagnostic apps](http://www.ncbi.nlm.nih.gov/pubmed/23325302); for [opioid conversion apps](http://www.ncbi.nlm.nih.gov/pubmed/23322549); and the [content of cancer apps](http://www.ncbi.nlm.nih.gov/pubmed/23275239).

### Group 2: Pro-Agility
The pro-agility camp, instead, are grounded in very practical matters. Most importantly, in practice, _software is not medicine._ Current clinical trial methods make testing health apps infeasible. The only way that I’ve seen trials being conducted is by means of a linear, incremental pipeline of steps that takes a drug from the lab (idea/phase 1) to the market (product/phase 4). This makes sense for drugs — in software terms, this is (roughly speaking) the [waterfall model](https://en.wikipedia.org/wiki/Waterfall_model) of development. Ask your developer friends what they think about that.

Developing and evaluating software linearly is not only expensive and ineffective, it also ties your evaluation to ‘bugs’ that could easily be fixed along the way… not to mention technology that will be outdated tomorrow:

> “Locking down these interventions locks in defects and eliminates the opportunities for quality improvement and adaptation to the changing technological environment, often leading to validation of tools that are outdated by the time that trial results are published.” (Mohr et. al, [JMIR](http://www.jmir.org/2015/7/e166/)).

### Some thoughts
There is a problem with the word _mHealth app_ — it is too broad. It currently includes health-tracking apps, health-calculator apps, health-information apps, health-diagnostic apps, and health-social networking apps. All of these are very different things.

There is a problem with the word _evaluate_ — it is too broad. For example, [this Twitter poll](https://twitter.com/NIHR_MindTech/status/672313228668588032) asked if robust user reviews were sufficient to evaluate a mental health app: the discussion includes (true) things like “users can’t evaluate the whether the app encrypts their data.” Determining that an app complies with certain engineering requirements such as encryption certainly doesn’t need a clinical trial.

One of the overriding problems here is that a health app’s outcomes fundamentally rely on its implementation. Usability matters; it may even trump evidence. Would you use an app that is based on the best evidence, but ugly as hell? Papers from clinicians (for example, [this one](http://www.ncbi.nlm.nih.gov/pubmed/24985342)) seem to recognise this, often claiming that their ‘future work’ (read: stuff that couldn’t work for this study) includes figuring out how to engage and retain users for a longer time. Are they best suited to be tackling design and engagement issues?

### Conclusion
It sounds like there is a lot more to this debate to unpack and discuss. Yes, there is insufficient evidence for mHealth app effectiveness. Yet our methods for generating evidence are ineffective. Yes, we need some kind of framework to build better mHealth apps; yet, we shouldn’t build an insurmountable wall of requirements that will prevent any innovation.

A highly compelling resolution that I’ve seen here advocates for a “to each to their own approach:” research should focus on _principles_, and let technologists take care of implementing them into engaging services:

> “We argue that RCTs will have greater scientific and public health value if they focus on the evaluation of intervention principles” (Mohr et. al, [JMIR](http://www.jmir.org/2015/7/e166/)).

This _seems_ sound, but — as above — fails to recognise that the success of some intervention principles may rely on their design/implementation.

There is a final, broader, point: I have yet to meet a patient who _really_ cares about statistical evidence and validation. Patients talk about ease of use, interactivity, engagement, and personal utility — things that are not measured (or statistically significant) outcomes of clinical trials; things that are developed by iterating and running A/B tests. Are patients therefore leaning towards the pro-agility approach? Perhaps the best we can do, for now, is follow the advice of the NHS Choices webpage:

> The only way to know if an app works for you is to download and try it.” ([NHS Choices](http://www.nhs.uk/Conditions/nhs-health-check/Pages/downloading-health-apps-safety-advice.aspx))

