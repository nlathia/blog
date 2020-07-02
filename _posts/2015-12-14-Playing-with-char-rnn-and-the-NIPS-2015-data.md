---
layout: post
title: Playing with char-rnn and the NIPS 2015 data
categories: [machine-learning, tools]
---

Since [samim](https://medium.com/u/f3c8148878e1) published all those awesome and fun posts on using a Recurrent Neural Network to generate text (see: [Zen-RNN](https://medium.com/@samim/zen-rrnn-on-meditation-machines-bbeb92aa62d3#.vn9ox6zb8), [TED-RNN](https://medium.com/@samim/ted-rnn-machine-generated-ted-talks-3dd682b894c0), [Obama-RNN](https://medium.com/@samim/obama-rnn-machine-generated-political-speeches-c8abd18a2ea0)), I’ve been looking for an opportunity to try the [char-nn](https://github.com/karpathy/char-rnn) library myself.

An opportunity came up after all of the papers at this year’s Neural Information Processing Systems Conference ([NIPS](https://nips.cc/) 2015) appeared online. What is more suitable to play around with* an RNN than a bunch of papers that talk a lot about RNNs?

### The NIPS 2015 dataset
The dataset was provided as part of this (kudos-only, curiosity-driven) [kaggle competition](https://www.kaggle.com/c/nips-2015-papers). It includes a CSV file that contains all of the papers’ text, extracted from the [papers accepted this year](https://nips.cc/Conferences/2015/AcceptedPapers).

### Preprocessing
The text data (extracted from the PDFs using the tool [pdftotext](https://en.wikipedia.org/wiki/Pdftotext)) is incredibly noisy: `pdftotext` does not extract page numbers, carriage-return broken words, equations, section headers, variables, figure captions, tables, footnotes, and all the other complexity the researchers can put together with various LaTeX commands into simple text. The result is text that we clearly do not want to train anything with. Here’s a relatively common looking example:

{% highlight js %}
pβ T xpiq ´ β T y pjq q2 Mij “ max<br>min β T WM β<br>(3)<br>βPB<br>||β||0 ďk
{% endhighlight %}

This text was “generated” by a human. For this experiment, I was looking to train using _full sentences_, which are actually a rarity in technical/algorithmic papers. They tend to (from my experience) be found in the Abstract, Introduction, Related Work, Discussion/Conclusion sections. I therefore filtered each paper — manually removing everything after the introduction and before the conclusion (but this varied between papers)— and yes, in case you were wondering, this was a huge pain and wasn’t done perfectly.

Note: I did try a bunch of regexs first (and used some to remove things like references), but could only get so far: there was a lot of variance in section headings, and in general too many cases/outliers/exceptions to automate this sensibly.

### Training
The char-rnn library makes life very easy. Once Torch (etc.) has been installed, and all the papers have been put into a single `input.txt` file, training the model is one line of code away.

I used the standard setting with a higher dropout value (0.5) to minimise overfitting — especially since the data remains quite noisy.

### Comparative, Nonsense Results
So here are some results. Example human entries come from picking a random line out of the input data that contains the seed text; machine text is then generated with the same seed.

**Seed: “The task we aim to solve”**

> Example Human: “The task we aim to solve is to fill in gaps of multiple consecutive data points in high-dimensional binary time series data.”

> Machine: “The task we aim to solve the proof of the inference learning problem provided by which and algorithms in the method methods are possible to the posterior and computationally of the state time to the demonstration are expected in constraints, in the local minimax.”

**Seed: “In this paper, we”**

> Example Human: “In this paper, we propose a novel parameter estimator for models on discrete space, which does not require calculation of the normalization constant.”

> Machine: “In this paper, we propose the input information of f is increased for each prediction and the minimax method is better than the following distributions and can be robust to analyze the tree image mint discretization to the computational experiments of s vret and only possible to be the acceleration rime in the linear regret in the possible data size.”

**Seed: “To conclude, ”**

> Example Human: To conclude, we show a set of optimized pseudo-inputs Z in black in Figure 1. We also superimpose the marginal of any single point in the realization, which is available through the spectral decomposition of L here .”

> Machine: “To conclude, the distribution constraints of each relative possible development decomposition of the submodular scale goal is the expected probability of the true of the recovery information in the prediction for parameters are step is the algorithm vore an analyzed method that this analysis after random nature by the wide procedure of maximum complexity sets.”

### Playful Seeds

**Seed: “The meaning of life is ”**

> Machine: “The meaning of life is shown to extend both similar state of our linear expectation in a computer variance of the level of produced controlled distributed the minimum truanding eigenvalue.”

**Seed: “Dinner tonight is ”**

> Machine:  “Dinner tonight is shown to extend a learning interest of size point of a function because then one been respectively.”

**Seed: “Machine learning research should ”**

> “Machine learning research should be generalized in networks of the case of the only quadratic shared images.”

**Seed: “My predictions are ”**

> “My predictions are related to the prior of the linear and non-not gradient function both from the model is to also consider the MDM of the posterior and define the true of the state that with large optimal analysis defined as the computation p.”

### Some thoughts
Machines aren’t going to be taking over anytime soon!

Extracting high-quality text from a PDF is harder than training an RNN (a.k.a. “machine learning,” as [this NIPS paper](https://nips2015.sched.org/event/5QRj/hidden-technical-debt-in-machine-learning-systems) points out, is a small part of “machine learning systems.”).

This is the first time that I play with the char-rnn library. It’s likely that people who know it a bit better could get better results. I was left wondering whether some further preprocessing could have made for a better training dataset. For example, some kind of clustering could have been used to identify papers that are similar (e.g., comparison over bags of words) — to then train only on one particular cluster.

With thanks to:
* Previous posts by [samim](https://medium.com/u/f3c8148878e1). 
* The authors and contributors of [char-nn on github](https://github.com/karpathy/char-rnn)
* Kaggle, for the data, and for being a generally awesome place. Found [here](https://www.kaggle.com/c/nips-2015-papers)

[*] This is **not** science.