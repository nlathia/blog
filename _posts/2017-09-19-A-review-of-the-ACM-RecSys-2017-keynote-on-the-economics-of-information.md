---
layout: post
title: A review of the ACM RecSys 2017 keynote on the economics of information
description: Four areas of research related to how we interact with information, and how that links to recommender systems.
categories: [conference]
---

The first keynote at the 2017 [ACM Conference on Recommender Systems](https://recsys.acm.org/recsys17/keynotes/#content-tab-1-0-tab) (RecSys) was by [Prof. George Loewenstein](https://www.cmu.edu/dietrich/sds/people/faculty/george-loewenstein.html).

In this wonderful lecture, Prof. Loewenstein delved into the past and present of the *economics of information* - using behavioural economics to study how we interact with the information around us. In a conference like RecSys, where most of the focus tends to be on the systems (data, algorithms, interfaces) that we can build to help people navigate information, it was refreshing to hear from a different discipline, that focuses on the ‘other’ end of the spectrum: people.

A number of weeks on, I’m revisiting the notes that I furiously typed throughout the session. Here they are, turned into sentences, with no guarantee that there are no mistakes, omissions, or misinterpretations.

The keynote began with a simple observation: there are people online who [rate toasters](https://www.amazon.co.uk/Toaster/b?ie=UTF8&amp;node=11716951). Who are these people? Why are they sharing this information? It may sound commonplace to rate something online, but when is the last time that you have gone to read toaster reviews?

What followed was a whirlwind tour of the history of this research. Starting as a counter to the ‘standard’ economic view of information gathering — i.e. that people are Bayesian, and rationally update their beliefs based on the information they have received — [behavioural economics](https://en.wikipedia.org/wiki/Behavioral_economics) research takes the view that people are motivated to hold certain beliefs because they feel good or fit into the sense they have (already) made of the world. The so-called *new economics* of information introduced a variety of concepts such as [adverse selection](href="https://en.wikipedia.org/wiki/Adverse_selection), [signalling](https://en.wikipedia.org/wiki/Signalling_%28economics%29), and [screening](https://en.wikipedia.org/wiki/Screening_%28economics%29).

The way we process information is biased: when faced with new information, rather than asking “should I believe this?” (is there valid evidence to support the information?) we tend to subconsciously ask “must I believe this?” (am I forced to change my beliefs) instead. We defend our beliefs in the same way that we defend possessions ([Abelson, 1986](http://onlinelibrary.wiley.com/doi/10.1111/j.1468-5914.1986.tb00078.x/full)). Our beliefs are not just shaped by evidence: for example, one study ([Kahan 2015](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2459057)) showed that believing in climate change was correlated with how ‘scientifically knowledgeable’ a person was. However, when the data was split by political affiliation, it showed an extreme result: those who were *more* scientifically knowledgeable but republican were *less* likely to say that there is solid evidence of recent global warming due to mostly human activity. So is the evidence driving the belief, or vice versa?

<img data-width="398" data-height="191" src="https://cdn-images-1.medium.com/max/800/1*CE-FzPxS69sOQUbLSKyYdg.png">

Screen shot from [Kahan 2015](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2459057), page 12.

One of the ways that Prof. Loewenstein proceeded to categorise *interaction with information* was via two axes: (1) whether we are *engaging* with (desiring) or *avoiding* the interaction, and (2) whether we are *obtaining* or *revealing* information. Combinations of these result in four areas of research:

## 1. Desire to Obtain Information (Curiosity)

Curiosity was broadly defined as the desire to obtain information. There’s an entire review of the field in *The Psychology of Curiosity* ([Loewenstein, 1994](https://www.cmu.edu/dietrich/sds/docs/loewenstein/PsychofCuriosity.pdf)); broadly speaking, it’s the desire for information *for its own sake*, as opposed to as a means to achieve a goal. We can be captivated by an intense desire for information — so intense that we can even seek out information when we know that finding it will bring us misery ([Hsee & Ruan, 2015](http://acrwebsite.org/volumes/1019134/volumes/v43/NA-43)). People tend to become more curious when they feel they are getting closer to information, and are most curious about themselves.

One item of discussion at the end of the talk was about how this research can speak to those designing systems: curiosity was pointed out as a factor that feels currently underutilised in the design of recommendation systems.

## 2. Avoiding to Obtain Information

There are many cases where we actively engage in avoiding information, even when it is “useful, free, and independent of strategic considerations” ([Golman et al., 2016](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2633226)). One reason cited for this is when we expect the information to be painful. For example, the paper states “if you know that your paper got rejected, you may still not want to read the reviewers’ comments.” That one brought back some memories for me!

An interesting domain where information avoidance is of particular significance is in financial investments — where it is known as the [ostrich effect](https://en.wikipedia.org/wiki/Ostrich_effect). In [Sicherman et al, 2016](http://www.cmu.edu/dietrich/sds/docs/loewenstein/FinancialAttention.pdf), a study is reported that looks at how investors are (not) logging into a portfolio information system as the market fluctuates. When the market declines, the log in rate drops by 9.5%; similarly, when the market was up (even if it was closed, such as on weekends), logins were higher.

This behaviour was also observed in a health domain: the higher a user’s [Vitality age](https://www.vitalitygroup.com/insights/what%C2%92s-your-vitality-age/) in an app (relative to their actual age), the more likely they were to click the ‘hide’ button on their Vitality age, and Loewenstein also mentioned the book [Don’t Even Think About It](https://www.amazon.co.uk/Dont-Even-Think-About-Climate/dp/163286102X), as discussing information avoidance in the context of climate change.

## 3. Desire to Reveal Information

Behaviours that seek to share information can be strange — we have websites where people can [confess](http://simplyconfess.com/) and share secrets, and the example used at the beginning of the talk (reviewing toasters) is just one of the many ‘weird’ things that people commit time to sharing information about.

Motivations to share information can be amazingly intense: it is no wonder that many metaphors (e.g. ‘bursting’ to tell someone) seem hydraulic in nature. One qualitative study of people who were ‘dying’ to share information (I missed the reference) reported that about half of them had something (mostly positive) to share about themselves, while the other half had something (mostly negative) to share about others.

One question that Prof. Loewenstein asked: why do we share so much information that is obvious? As an example, telling someone that Lake Como, the location of the conference, is beautiful. What has been gained from that revelation?

This line of research links directly into recommender systems that rely on crowdsourced information. Why are users providing reviews? Perhaps they are motivated by reciprocity with (or revenge on) providers, showing off their sophistication (bragging), they may want to influence others (e.g., based on how many people will see their review), or just want to be noticed (e.g., the prank on YouTube by the couple that wanted to boost their profile which [ended up as a fatal shooting](http://www.telegraph.co.uk/news/2017/06/29/us-woman-shoots-boyfriend-dead-youtube-stunt-boost-online-profile/)). This matters because it determines selection bias — who is going to write the reviews — as well as providing effective strategies to solicit reviews.

(Wait, why am I writing this blog post again? Oh yeah, for the [claps](https://help.medium.com/hc/en-us/articles/115011350967-Claps) 👏!)

## 4. Avoiding Revealing Information (Privacy)

Privacy, in the context of this talk, was defined as the desire to not share information.

<img data-width="398" data-height="191" src="https://cdn-images-1.medium.com/max/600/1*wvkG720g7pIpbtk9nymcPg.jpeg">

The ‘standard’ economic model of privacy tended to model this as an optimisation — weighing the costs vs. the benefits of revealing something. However, our preferences are often ill defined, as is our ability to navigate the cost/benefit of information disclosure in different contexts.

While a large majority of people believe that privacy policies are put in place to protect them, users are often unaware as to the real implications. Case in point: [this study](https://spectrum.ieee.org/tech-talk/telecom/security/-users-sign-over-first-born-for-free-wifi), where users signed over their first born child when agreeing to the terms to join a free Wi-Fi network.

Our willingness to share information depends on arbitrary cues from our current context (example: strangers on a plane) rather than the net benefits of sharing information ([Acquisti et al., 2011](https://www.cmu.edu/dietrich/sds/docs/loewenstein/StrangersPlane.pdf)). People take cues about what to reveal from others and their own past experiences ([Acquisti et al., 2012](http://repository.cmu.edu/heinzworks/342/)). More online control for privacy makes people more likely to reveal information about themselves ([Brandimarte et al., 2013](http://www.heinz.cmu.edu/~acquisti/papers/acquisti-SPPS.pdf)); put another way, the more you can control your privacy online, the “more rope you have to hang yourself with.” People exhibit default default effects and loss aversion with respect to their privacy ([Acquisti et al., 2013](https://www.cmu.edu/dietrich/sds/docs/loewenstein/WhatPrivacyWorth.pdf)).

The main take-aways seemed to be: people don’t seem to care much about privacy, except when you ring alarm bells; privacy preferences are context-dependent, and there are a host of research questions that remain at the intersection of privacy and information disclosure (why do people review strip clubs? Or gastroenterologists? He gave examples of both!).

## In Closing

It looks like [this recorded talk of his is similar](https://www.youtube.com/watch?v=o55GZt_sII4) if you want to dive in deeper. When announcing the [best paper award](https://recsys.acm.org/best-papers/) this year, the award committee made a special point of calling out for more research (like this year’s best paper) that builds off of theories developed in other disciplines into practical impact in the recommender system domain.

The same can be said for most disciplines — there are some wonderful things to be learned and explored if you step away from home from time to time. And this keynote was one of those times for me.