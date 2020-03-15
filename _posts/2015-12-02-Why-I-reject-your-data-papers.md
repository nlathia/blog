---
layout: post
title: Why I reject your data papers
description: I believe that research papers are innocent (“accept”) until proven guilty (“reject”). Here are some crimes that move your data mining paper into the guilty camp quite quickly, and make me feel sad! 
categories: [opinion, research]
---

*No baseline.* You present results of your wonderful new machine learning algorithm compared only to variations of itself (e.g., results with different parameters). No baseline or alternative approach results presented. Quantitative results on data only make relative (not absolute) sense: If your algorithmic magic is 99% accurate, is a simple linear regression baseline 98% or 58% accurate? There’s a huge difference here.

*Excessive/unmotivated data pruning.* You prune your dataset to fit your particular use case, and do not give any concrete motivation for doing so. The most common of these is the “we removed all of the users with fewer than X entries,” with no explanation as to why (overall), why X rather than some other value, what effect this had, or what would happen to users who do not have X amount of data.

*Your metrics aren’t measuring your claims.* You’re claiming an increase in user satisfaction (or long-term engagement, or recommendation quality), but are only measuring prediction accuracy.

*You’ve evaluated your algorithm manually.* You evaluate your algorithm by manually inspecting test instances. “Hey look! It works for these two cases, so surely this is cool!” I admit that I have sometimes done this while debugging algorithms, but (as far as I recall) I have never reported results in this way. If you’re going to do this, you need to (a) not be the one evaluating (i.e., run a user study), and (b) do this on way more than the handful of instances that you can do. 

You do more than one thing on this list.

And some minor issues to keep in mind:
* I haven’t read every paper out there, and assuming that I’m going to read all of the papers you cite (“we do what [76] does.”) is a stretch.
* Related work should discuss _why work is related_. It’s called “Related” work, and not “Here’s a list of what others have done”
* As a reader, I will skip your “the rest of this paper is organized as follows” paragraph. I don’t care much about knowing how your paper is structured; I would much rather you spend the time/space telling me about what you have found and its implications.
* While I know that some people feel very strongly about this, I don’t care if your related work is Section 2, or Section [Conclusion -1] as long as it is readable _where it is_. Are you discussing related work results compared to yours? That should come after you have presented your results. Are you discussing related work as a motivation for yours? Perhaps that should appear after the Introduction.

Update: [here](https://www.brainpickings.org/2014/03/28/daniel-dennett-rapoport-rules-criticism/) is an interesting suggested code of conduct for reviewing (and criticizing) research.
