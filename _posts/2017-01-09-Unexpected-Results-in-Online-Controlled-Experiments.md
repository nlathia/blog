---
layout: post
title:  "Unexpected results in online controlled experiments"
tags: [paper]
excerpt_separator: <!--more-->
---

“Getting numbers is easy, getting numbers you can trust is quite difficult.”
<!--more-->

While discussing some A/B tests that we are running, [Colin McFarland](https://medium.com/u/e0e17553ea9f) recommended “Unexpected Results in Online Controlled Experiments” by Kohavi and Longbotham, from SIG KDD Explorations 2010. Following on from my other research summaries, here’s a quick overview of the paper.

```
If a result seems unexpected it may be due to lack of understanding of user behavior or it could be due to a software or experimental design problem.
```

This short, well-written paper does not focus on specific ideas or experimental results, but instead covers a wide range of unexpected lessons learned across a variety of online experiments. Many of these lessons capture differences between the control (A) and treatment (B) groups that end up unduly affecting your experiment, causing you to measure a change where none should have been measured.

## System Differences
The first type of differences are “under the hood:” system differences that you may not think about when running an experiment. There were two examples where the website’s speed was unintentionally affected in one of the two groups, resulting in performance differences, another example where a monitoring system skewed the metrics, and an example where website localisation was not taken into account properly:
1. Often, “if the randomization function determines that the user should be in Control, the page is displayed; if the randomization shows that the user should be in Treatment, a browser redirect is done.” The redirect added loading time to the treatment: users in B had a slower experience.
2. In another example, an experiment was run where both the control and treatment website variants made use of a cache. Since the experiment was set up as 90% (A) and 10% (B), the cache was skewed towards those users/entries that were in the control — resulting in faster performance for A.
3. In a one-click conversion experiment, there was a surprisingly large number of page views in the treatment group. “An investigation revealed that the experimenting site had a monitoring system that requested the page and then simulated a click on the purchase button and checked the ordering pipeline” — the difference was due to the monitoring system, not the users.
4. In practice, you want the distribution and types of users arriving into your control and treatment groups to be the same. Do your experimentation tools take into consideration how your website may be localised across users? “When a new version of the MSN US Home Page was tested in a controlled experiment […] the results were highly biased because the population of users from non-US IPs was much higher in the Treatment than in the Control.”

## Experiment Execution
There were other unintended differences that seemed to originate from how the experiments were executed and measured.:
1. In one experiment, the percentage of users allocated to the treatment and control was ramped up over time: “since the Treatment had a much larger percentage on the last two days, the clicks per user on those days carried larger weight with the Treatment mean making the Treatment look worse than the Control.” This lead to an example of [Simpson’s Paradox](https://en.wikipedia.org/wiki/Simpson%27s_paradox).
2. When evaluating email campaigns, experiments were run by sending emails to a treatment group, and withholding them from the control group. “The OEC (Overall Evaluation Criterion) for the campaigns was based on purchases whose sessions were referred by the e-mails. But under this criterion, all ideas/campaigns evaluated positively.” The effect? Users started complaining about receiving too many emails.

## Conclusion
The main recommendation in the paper is to run A/A tests, which tests the tool(s) and methods that you use to split users across variants. An A/A result should show no significant difference (you can expect a difference 5% of the time even in A/A tests if alpha is 0.05 !), but often uncovers some: “the slightest difference could result in significant changes to the user experience.”

The authors also share the following take-aways:
1. Always get information on the ultimate action you want the user to take.
2. Beware of classical statistical formulas that assume independence.
3. Experimental control is critical. Keep everything constant except the thing you want to test.
4. Drill-down by time to look at hourly data. Had the result not been so surprising (e.g., if the treatment were 2% worse), we might have accepted the result. We now regularly show hourly plots for sanity checks to detect such anomalies.
5. Use screen scrapers to save screen shots of the pages being experimented on a regular basis in order to allow debugging of surprises. We have found this to be extremely useful in other experiments.
6. To avoid Simpson’s Paradox, “you can simply require that the percentage of users in the Treatment relative to the Control not change during the experiment.”

At Skyscanner, our internal experimentation tool (Dr Jekyll) simply forces a new allocation and new scorecard anytime the experimenter makes a change. We also force buckets to be balanced: you choose your percentage of users you want in the experiment and we split all treatments equally.

```
Running a good online experiment is a lot more than just randomly assigning users into two groups — it requires careful planning and vigilance in monitoring for known and yet-to-be discovered sources of experimental bias.
```

Here’s another good read about experimentation: [Design Like You're Right, Test Like You're Wrong](http://codevoyagers.com/2016/03/16/design-like-youre-right-test-like-youre-wrong/).