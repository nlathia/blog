---
layout: post
title: Machine Intelligence Summit 2017
description: Overview of #reworkMI in Amsterdam.
categories: [conference]
---

Last week, I was at the [4th Machine Intelligence Summit](https://www.re-work.co/events/machine-intelligence-summit-amsterdam-2017) #reworkMI in Amsterdam. The mission of the event is:

> “…to bring together government officials, academics, entrepreneurs, investors, and industry professionals all in the same room to discuss the latest breakthroughs in machine learning, deep learning and AI and to explore their applications in industry and their potential impact…”

When I ran a quick hand-raising survey of the room on the 2nd day, it looked like the audience was mostly from an academic or startup background (I admit, I forgot to ask about government officials).

The full schedule and abstracts are [available online](https://www.re-work.co/events/machine-intelligence-summit-amsterdam-2017/schedule), and I’m writing this summary by looking back there. Here’s a brief overview of the talks, grouped by themes (rather than by speaker order):

## Computer Vision

**[Damian Borth from DFKI](http://www.dfki.uni-kl.de/~borth/)** discussed explorations of the latent space created by Generative Adversarial Networks (GANs) in terms of adjective-noun pairs to capture subjective traits of images. The results that were presented included a way of translating images to better capture some traits; e.g., translating an image from a “foggy sky” to a “pretty sky.”

**[Appu Shaji from EyeEM](https://www.eyeem.com/blog/meet-the-team-appu-shaji/)** discussed how [EyeEM](https://www.eyeem.com/) is using computer vision to create new experiences for customers. One interesting application that he discussed was the the ‘EyeEM Selects’ feature (see this [blog post](https://petapixel.com/2017/06/23/eyeem-selects-will-show-best-photos-phone/)) which picks the best photos from your library to showcase.

## Fashion & Beauty
[Panagiotis-Alexandros Bokaris](https://scholar.google.com/citations?user=o8jFbhIAAAAJ&amp;hl=en) from L’Oréal promised us a talk that would ‘not just be about lipstick.’ His talked covered a wide breadth of work being done at the intersection of augmented reality and computer vision — with small pointers about how these are making their way into beauty-related products, like [smart hairbrushes](http://www.loreal.com/media/press-releases/2017/jan/kerastase-and-withings-unveil-worlds-first-smart-hairbrush-at-ces-2017) (and lipstick).

**[Roland Vollgraf](https://research.zalando.com/welcome/team/roland-vollgraf/)** from [Zalando Research](https://research.zalando.com/) discussed applications of deep learning to items of clothing. By learning from product images, textual descriptions, and vendor attributes, they created a ‘fashion DNA,’ which can be applied to cluster items as well as to generate items of clothing that don’t exist.. yet.

## Search & Recommendation
**[Aleksandr Chucklin](https://scholar.google.com/citations?user=oCOhXmAAAAAJ)** from Google Research gave a presentation about the evaluation of search engine result pages (Arxiv paper [here](https://arxiv.org/abs/1609.00552)). This is another talk that demonstrates how nuanced the evaluation of search results has become — there is research on the differences in search between mobile/web, on evaluating ‘good abandonment’, and on evaluating the quality of mixed-content and non-linear search result pages.

I gave a talk about bootstrapping the destination discovery system at Skyscanner, and was perhaps one of the few not discussing an application of deep learning (at least, for now!). More on that to come in a different blog post. I think that this was my first ever [appearance on Periscope](https://twitter.com/teamrework/status/880006042985181185).

## Shopping & Auctions
**[Daniel Gleber from Picnic](https://www.picnic.nl/)** gave an overview of the various problems that Picnic (a Dutch online shopping service) is tackling from a machine learning perspective: they span the entire customer journey, and more. For example, this included scaling a customer feedback system, where users can request items that are not currently stocked, from being a Slack channel into an automated classification system. Another focus area was on predicting delivery times — the y-axis of the graph with error metrics was in seconds!

<blockquote class="twitter-tweet"><a href="https://twitter.com/gr33ndata/status/880389787810754560"></a></blockquote><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

**[Peter Tegelaar](https://twitter.com/ptegelaar)** from [Catawiki](https://www.catawiki.com/) discussed a variety of applications of deep learning in Catawiki’s auction system. This ranged from using classification to support product categorisation through to estimating item’s prices. Peter also spoke in an open way about an application of deep learning that didn’t work for them (estimating customer lifetime value), and warned us about treating everything as a nail, now that deep learning is the latest hammer.

## Robotics & Hardware
**[Prof Sven Behnke from the University of Bonn](http://www.ais.uni-bonn.de/behnke/)** discussed applications of deep learning for robot cognition. There are various angles to this space. One of the many interesting applications he discussed was using these methods in the [RoboCup 2016](http://www.robocup2016.org/en/events/amazon-picking-challenge/), where participants competed with robots tasked with automated picking in an unstructured environment (see [Amazon Robotics Challenge](https://www.amazonrobotics.com/#/roboticschallenge)).

<blockquote class="twitter-tweet"><a href="https://twitter.com/im_td/status/880394505752588292"></a></blockquote><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

**[Theo Gevers from the University of Amsterdam](https://staff.science.uva.nl/th.gevers/)** gave an overview of his group’s research into automatic object detection, segmentation, and reconstruction. He gave a demo of an application that can automatically infer a person’s emotion, age, and gender from a live video stream. No surprise that the first question was about privacy.

**[Adam Grzywaczewski of NVIDIA](https://www.crunchbase.com/person/adam-grzywaczewski)** gave an overview of the infrastructure challenges that are arising as a result of the surge of interest in deep learning. One question he got was about how NVIDIA’s GPUs compare to [Google’s TPUs](https://en.wikipedia.org/wiki/Tensor_processing_unit). His answer? I don’t know, I can’t go out and buy Google’s board to compare it to ours.

<blockquote class="twitter-tweet"><a href="https://twitter.com/maria_axente/status/880466809178259456"></a></blockquote><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

**[Ben Scott-Robinson](http://cargocollective.com/benscottrobinson)** from [The Small Robot Company](https://www.smallrobotcompany.com/) gave a talk about three robots: Tom, Dick, and Harry, and how they are being designed to help farmers with everything from soil analysis to planting seeds. There was a fun focus on anthropomorphising the robots that are being designed and it will be interesting to see this ‘farming as a service’ model evolve.

**[Dor Kedem](https://nl.linkedin.com/in/kedemdor)** from [ING](https://www.ing.nl/particulier/index.html) gave a talk about resource allocation in large computing clusters, and how using machine learning with cluster utilization patterns can save significant amounts of money, that were otherwise being spent to allocate resources that end up sitting idle.

## Document Analysis & Embeddings
**[Sotirios Fokeas from Swisscom](https://ch.linkedin.com/in/sotirios-fokeas-2630a9a6)** gave a talk about using sentence embeddings to extract high coverage key-phrases in documents. Sentence embeddings are an interesting approach that has also received much recent attention.

**[Marcus Liwicki from the University of Kaiserslautern](http://blog.mindgarage.de/)** gave an overview of methods that have worked well in analysing documents. This includes the renown Long Short-Term Memory (LSTM) networks.

LSTMs also made an appearance in **[Mihai Rotaru’s](https://scholar.google.nl/citations?user=DkZNtf8AAAAJ&amp;hl=en)** talk about [Textkernel](https://www.textkernel.com/); he gave a talk about using deep learning to automatically parse data from candidate’s CVs, and using this data to match applicants with jobs. This included considering language-independent models, and finding job-title similarities via embeddings.

## Data Analysis with Machine Learning
**[Vahid Moosavi from ETH Zurich](https://vahidmoosavi.com/)** gave a talk about using data to model urban phenomena, like real estate prices and air pollution flow. One of the key points that he made is that the application of data and machine learning to problem spaces that traditionally relied on domain knowledge is shifting the definition of ‘expertise’ from _having the answers to known questions_ towards _knowing to ask good questions_ (and whether they can be answered with data).

**[Ingo Waldmann from University College London](http://zuserver2.star.ucl.ac.uk/~ingo/index.html) gave a presentation about using deep learning to automate the analysis and discovery of extrasolar planets (see ‘[Dreaming of Atmospheres](https://arxiv.org/abs/1511.08339)’), and how astronomy is the latest victim in the growing domains of research where data availability outweighs our ability to analyse it.

## Augmenting Experts with Deep Learning
**[Tijmen Blankevoort from Scyfer](http://scyfer.nl/team/tijmen-blankevoort/)** talked about human-in-the-loop or active learning. This included applications in medical imaging.

**[Larens Hogeweg](https://scholar.google.com/citations?user=hg2WQI8AAAAJ&amp;hl=en)** from [COSMONiO](http://www.cosmonio.com/) also discussed applications of deep learning to medical imaging. In particular, he was focusing on learning from ‘small data’ (even 1 or 2 samples), and demonstrated how approaches seem to generalise well.

## Bayesian Methods
**[Christopher Bonnett](https://twitter.com/cbonnett)** from [alpha-i](https://alpha-i.co/) presented about using Bayesian approaches in deep learning to expose a model’s uncertainty in its predictions, alongside the predictions themselves.

<blockquote class="twitter-tweet"><a href="https://twitter.com/ptegelaar/status/880400926225387521"></a></blockquote><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

**[Laith Alkurdi](https://scholar.google.com/citations?user=nQ86angAAAAJ&amp;hl=en)** from [Freeletics](https://www.freeletics.com/en/) gave a talk about how Bayesian methods are being used to model users and create a ‘coaching AI’ in the freeletics app.

## Panel Discussions
The first day’s panel focused on investing into AI. The panel was moderated by Matthew Bradley ([Forward Partners](https://forwardpartners.com/team/)), and included Julius Rüssmann ([Earlybird Venture Capital](https://www.earlybird.com/)), Carlos Eduardo Espinal ([Seedcamp](http://seedcamp.com/team/carloseduardoespinal/)), and Daniel Gleber ([Picnic](https://www.picnic.nl/)). Interesting snippets included assessing startup’s competitive advantage in terms of _access to training data_, and balancing between generalist AI startups and those that grow in a specific vertical.

<blockquote class="twitter-tweet"><a href="https://twitter.com/HarrmanCyber/status/880070066401759233"></a></blockquote><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

The second panel was a bit of a gear-shift compared to the rest of the conference, and focused on responding to growing threats in cyber security. The panel was moderated by Eelco Stofbergen ([CGI](https://www.cginederland.nl/expert/eelco-stofbergen)) and included Tanya Harris ([Harrman Cyber](https://www.harrmancyber.com/)) Ian Bryant, ([Trustworthy Software Foundation](http://tsfdn.org/)) and Colin Williams ([University of Warwick](https://www2.warwick.ac.uk/fac/sci/wmg/research/csc/people/cwilliams/)). An interesting question that the panel received was about whether the ‘good’ or the ‘bad’ guys were making better use of AI in securty attacks/defenses: the answer was along the lines of ‘we’ll have to wait and see.’

## Closing Thoughts
#reworkMI was an interesting conference, and I met a bunch of great people. Everyone is excited about deep learning — no surprises there — and that excitement was mostly presented from a very technical perspective. However, many of the speakers had slides about the challenges that they currently face in their deep learning systems (like the one below): these spanned the entire product life cycle. There is still much work to be done.

<blockquote class="twitter-tweet"><a href="https://twitter.com/neal_lathia/status/880355265861877761"></a></blockquote><script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>