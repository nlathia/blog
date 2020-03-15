---
layout: post
title: Share data or perish
categories: [opinion]
---

The [International Committee of Medical Journal Editors](http://www.icmje.org/) is [seeking feedback](https://forms.acponline.org/webform/comments-icmje%E2%80%99s-proposals-sharing-clinical-trial-data) on their proposal ([pdf](http://icmje.org/news-and-editorials/M15-2928-PAP.pdf)) to require authors to share the deidentified individual-patient data, or IPD, that was used to generate research results “as a condition for consideration for publication.” The motivation behind this is to foster a culture of reproducibility:

> “The data underlying the results are defined as the IPD required to re-produce the article’s findings, including necessary metadata.”

I was interested in this proposal since data sharing is already widely practiced in Computer Science research. For example, see the [ICWSM datasets](http://icwsm.org/2013/datasets/datasets/), the [CRAWDAD repository](http://www.crawdad.org/), or the annual [ACM KDD Cup](http://www.kdd.org/kdd-cup).

In general, data sharing is something that I strongly agree with. In fact, my PhD research was made possible by the data shared as part of the [Netflix Prize](http://www.netflixprize.com/) and the [MovieLens project](https://movielens.org/).

So how could clinical trial data be any different? This post shares the feedback that I submitted.

## 1. Sharing data as a condition for publication
**tl;dr: My main concern is with the term “de-identified individual” data.**

In August 2006, [AOL shared a large dataset](https://en.wikipedia.org/wiki/AOL_search_data_leak) of “de-identified individual” query logs for research purposes. Although people were identified in the data by an anonymous identifier, individuals could be located by cross-referencing queries with public phonebook listings. In October 2006, Netflix released a large dataset of “de-identified individual” movie ratings as part of a [research competition](http://netflixprize.com/). Again, people were identified by an anonymous identifier, but individuals could be identified by cross-referencing queries with public IMDB ratings (which [led to a law suit](http://www.wired.com/2010/03/netflix-cancels-contest/)).

It is clear that the utility of sharing a dataset is for other researchers to be able to replicate and build on findings. While there are techniques for anonymizing data to a certain degree (e.g., [k-anonymity](https://en.wikipedia.org/wiki/K-anonymity)) it is not clear that using these will still allow research findings to be replicated. Further, sharing data as a “requirement” for publication seems to shift the incentive towards sharing data quickly, rather than sharing data safely.

## 2. Sharing data within 6 months
**tl;dr: 6 months is an incentive for sharing quickly, not safely.**

As above, a possible concern is that the time frame requirement will push researchers to share the data, rather than take the necessary steps to share it safely.

## 3. Planning to share data when registering a trial
This point seems obvious. I didn’t leave any feedback here (also, I’ve been prevented from sharing data from systems I’ve created since I didn’t get consent about data sharing at the outset).

## 4. Providing credit to those who share data
**tl;dr: This should not be a problem. The academic community operates on citations.**

Computer Scientists very frequently collect research data from public sources (e.g., social media) and/or write software that they use in their research. Many of them publish a finding and then release some data — with an open (unenforced) request that should the data be used for further research, their original publication should be cited.

More details: See how the hugely popular scikit-learn project [asks for credit here](http://scikit-learn.org/stable/about.html#citing-scikit-learn). Also, “research parasites” is one of the most ridiculous terms I’ve come across recently.

## 5. Other Comments
**tl;dr: What about the edge cases? Do participants opt-in, or opt-out?**

There are some cases that are worth considering.
- Is the data still useful if, once appropriately “de-identified,” research findings cannot be replicated?
- What if an insufficient number of trial participants give consent for their data to be shared (i.e., the data that can be shared does not allow for research findings to be replicated)?
- What will happen to trial recruitment when prospective participants are told that their data will be shared?
- Perhaps most importantly, should researchers be denied publication if they cannot share their data?

## And I clicked submit. Some final thoughts.
I don’t think that the data sharing debate is going to be concluded by putting strict data sharing rules in place: the way this proposal is currently framed sounds like a recipe for disaster. If sharing users’ 1–5 stars put Netflix in court, what will happen when we share clinical trial data?

More broadly, those advocating for sharing data seem to be doing so because that is the only way they see that certain things (e.g. reproducing findings) will be allowed to happen. Would data sharing practices still be enforced if we had the right tools that allowed us to reproduce findings without having to download a dataset?

Kaggle, for example, already has a tool (called [scripts](https://www.kaggle.com/scripts)) that allows data scientists to run “one-click analyses” without any data download. This is the right direction to go: What we really need (and what some smart researchers [are working on](http://arxiv.org/abs/1501.04737)) are tools that allow for data to be safely queried. Until those tools see the light of day, all data sharing enforcement is going to do is put us (as researchers) and trial participants/system users (as data points) into murky and dangerous waters.
