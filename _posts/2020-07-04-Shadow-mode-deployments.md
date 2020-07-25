---
layout: post
title: "Shadow mode deployments: somewhere between \"on\" and \"off\""
description: This is one of the most useful tools that I know of to quickly ship systems that make important decisions with high uncertainty.
categories: [data-science, tech-lead]
---

<meta charset="utf-8">
<title>Redirecting to https://nlathia.github.io</title>
<meta http-equiv="refresh" content="0; URL=http://nlathia.github.io/2020/07/Shadow-mode-deployments.html">
<link rel="canonical" href="http://nlathia.github.io/2020/07/Shadow-mode-deployments.html">

Shipping changes to a system that makes a complex decision is rife with uncertainty. You may be adding a rule to a rule engine that already has 10s of rules, you may be changing a machine learning model that eats up 10s of features and spits out a probability, or (more likely) you may be doing a combination of both.

The reason this is often difficult is because the most pressing question that comes up is no longer just "will it work?" (as in, will the system make **a** decision?) but, rather, "will it work well?" (i.e., will the system make **a good** decision?).

I've seen these types of cases lead to a lot of worry and _analysis paralysis_: teams do not ship because they are worried that their system will make _bad_ decisions, and instead opt to try and map out 'what if' scenarios on paper or reconstruct a lot of possible scenarios using historical data.

To get around this, I'm a vocal proponent of shipping systems in one of three states: off, on, and shadow mode. Shadow mode, specifically, is the best tool I've found to answer the 'how good _could_ this system be?' question. So, what are these three states?

### ❌ Off 

A system that is "off" doesn't do anything _except for_ integrate well into the rest of the system. If called, it doesn't execute anything and returns a default value without breaking.

It seems odd to think about shipping code that doesn't do anything. As Stephen's post on [High Growth Engineering](https://highgrowthengineering.substack.com/p/writing-maintainable-code-at-speed) explains, this is a useful way to define interfaces between different systems and enable each subsystem to be iterated on separately.

I've seen this approach used most often when wanting to ship a feature that impacts many teams and one team (or group of stakeholders) isn't ready. Instead of waiting for them, ship it and turn it off for them! Having the ability to quickly flip the switch and turn things off is also a life saver if you are dealing with an incident.

### ✅ On

Systems that are "on" are just that: when called, they do some work, they log some data, and they return a value.

It is common to add conditions here that quantify _how_ on some systems are. This could be for two reasons:

1. **Staged roll out**: you may be unsure of the performance of a system, so you just turn it on for a small fraction of traffic (e.g., 5%). As nothing blows up, you continue to turn up the dial until you reach 100% and the system is generally available for all your customers.
2. **Experiment**: you may want to compare two variants of a system, and so you redirect a percentage of traffic down one decision path, and the remainder goes down another. This is different from a staged roll out because it ends with the winning variant being turned on for everyone.

Both of these typically manifest in the same way--using flags of some sort in the code--but have very different intents and outcomes (safely scale up vs. compare and decide). One of my bugbears is when teams call something "an experiment," when it's actually a staged roll out (but I will leave that rant for another day).

### ⚠️ Shadow mode

Finally, systems that are in "shadow mode" are somewhere in between: when called, they do some work, they log data about all of their decisions, and then return a default value _as if_ they were off. All of the work is done, but the decision is not acted on.

You end up with is a boat load of data that you can use to pragmatically answer the question: _what if this system had been on?_ (because, technically, it was on!) without requiring Data Scientists to try and reverse engineer answers to this out of historical data. Shadow mode balances between insight (you get all of the data) and risk (you don't need to act on the outcomes); critically, it tests your system's performance on real, recent examples.

Last year, we were shipping some text classifiers. We had trained them using historical data that had been manually tagged (and we knew the tags were _sometimes_ unreliable); we were unsure of how well classifiers trained on old data would perform on more recent conversations. Given that these classifiers were powering a system that sent automated answers to customers, we were not willing to turn them on without some reassurances that they would work well.

We had a couple of options: (1) wait and collect new data, and then manually evaluate the classifiers using it, or (2) ship the classifiers _in shadow mode_.

We did the latter; we shipped a system that would query the classifiers for their decision, log the result, and then _carry on as if it never had_. In this case, the automated answer was not sent to customers _even when_ when the classifiers said that one should be sent. Instead, for a sample of these decisions, the system created validation tasks which would appear in our internal tooling. We used them to manually evaluate those decisions. The result? Within days, we had precision and recall values for the classifiers--based on the live data from production. This enabled us to make quicker decisions: to either confidently turn them on, or know that we needed to do more work and retrain the models.

### 💭 How does this compare to A/B tests?

The one bit that you may be thinking about is how _shadow mode_ compares to A/B testing. There are _different_ problems that shadow mode deployments enable solving: shadow mode allows you to know whether a system inherently works before you go ahead and experiment with whether it impacts a business metric.

For the text classifier example, above, we wanted to be sure that the system worked well (it gave precise answers) _before_ testing whether it impacted customer behaviour as we hoped it would (sending automated support answers would encourage self-service).

There are other scenarios where regulatory guidance may impede running A/B tests (I imagine this is the case with credit scoring models), or where you have low-volume/high-impact traffic (as is the case in fraud monitoring) where you may not _want_ to send a percentage of traffic down a path that you don't know anything about.

I'm writing this post as it has become one of the most recurrent things that I've talked about with Engineers when we are shipping systems that make a decision. Many times, those decisions are powered by a machine learning model; but the principle applies equally if you remove machine learning from the equation.

