---
layout: post
title: "Shadow mode decisions: somewhere between \"on\" and \"off\""
description: This is the most useful approach to ship systems that make important decisions with high uncertainty
categories: [data-science, tech-lead]
---

Shipping a system that makes a complex decision is rife with uncertainty. You may be adding a rule to a rule engine that already has 10s of rules, you may be shipping a machine learning model that eats up 10s of features and spits out a probability, or (more likely) you may be doing a combination of both.

The reason this is often difficult is because the biggest question that comes up is no longer "will it work?" (as in, will the system make **a** decision?) but, rather, "will it work well?" (i.e., will the system make **a good** decision?).

The classical answer that you will hear - from Data Scientists and Product Managers alike - is to resolve all uncertainty via experimentation: by running some form of A/B test. While I tend to wholeheartedly agree with this, there are specific circumstances where an A/B test is the pragmatic choice, and there are many cases where you may need some insight about how the system _could_ perform before this.

I've seen these types of cases lead to a lot of worry and _analysis paralysis_: teams do not ship because they are worried that their system will make _bad_ decisions, and instead opt to try and map out 'what if' scenarios on paper or using historical data.

To get around this, I'm not a vocal proponent of shipping systems in one of three states: off, on, and shadow (or dark) mode. Shadow mode, specifically, is the best tool I've found to answer the 'how good _could_ this system be?' question. So, what are these three states?

### ❌ Off

A system that is "off" doesn't do anything _except for_ integrate well into the rest of the system. If called, it doesn't execute anything and returns a default value without breaking.

It seems odd to think about shipping code that doesn't do anything; as Stephen's post on [High Growth Engineering](https://highgrowthengineering.substack.com/p/writing-maintainable-code-at-speed) explains, this is a useful way to define interfaces between different systems and enable each subsystem to be iterated on separately.

I've seen this approach used most often when wanting to ship a feature that another team (or group of stakeholders) wasn't ready for. Instead of waiting for them, ship it and turn it off!

### ✅ On (or partially on)

Systems that are "on" are just that: when called, they do some work, they log some data, and they return a value.

It is common to add conditions here that quantify _how_ on some systems are (or _who_ is it on for). This could be for two reasons:

1. **Staged roll out**: you may be unsure of the performance of a system, so you just turn it on for a small fraction of traffic (e.g., 5%). As nothing blows up, you continue to turn up the dial until you reach 100%.
2. **Experiment**: you may want to compare two variants of a system, and so you redirect a percentage of traffic down one decision path, and the remainder goes down another. 

Both of these typically manifest in the same way--using flags of some sort in the code--but have very different intents and outcomes (safely scale up vs. compare and decide).

### ⚠️ Shadow mode

Finally, systems that are in "shadow mode" are somewhere in between: when called, they do some work, they log data about all of their decisions, and then return a default value _as if_ they were off. All of the work is done, but the decision is not acted on.

What you end up with is a boat load of data that you can use to answer the question: _what if this system had been on?_ (because, technically, it was on!) without requiring Data Scientists to try and reverse engineer answers to this out of historical data that is available.

Shadow mode balances between insight (you get all of the data) and risk (you don't need to act on the outcomes); critically, it tests your system's performance on real, recent examples.

#### 🤖 A simplified example



### 💭 Wrapping up

I'm writing this post as it has become one of the most recurrent things that I've talked about with Engineers when we are shipping systems that make a decision. Many times, those decisions are powered by a machine learning model; but the principle applies equally if you remove machine learning from the equation.

If I've sent this blog post to you after a 1:1 and you've reached this far, let me know--I owe you a coffee.

