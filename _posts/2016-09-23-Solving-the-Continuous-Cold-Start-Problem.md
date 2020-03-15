---
layout: post
title: Solving the continuous cold start problem
description: A review of a paper by Kiseleva, Tuzhilin, Kamps, Mueller, Bernardi, Davis, Kovacek, Einarsen, Hiemstra.
categories: [recsys, paper]
---

### Background
Data Scientists across Skyscanner have started meeting every fortnight to discuss research papers that tackle similar problems to those that we face within Skyscanner. We kicked off in late August with [this paper](https://arxiv.org/abs/1607.07904) from Booking.com and NYU: “Solving the Continuous Cold Start Problem in E-commerce Recommendations”. As part of the reading group, we’re also writing up a brief, non-technical overview the problems/opportunities we discussed.

### The Problem: “Cold Users”
In many systems across the web, services would like to recommend interesting things (movies, music, restaurants, or travel destinations). How do you recommend something to a user that you have never seen before?

Researchers call this the [cold start](https://en.wikipedia.org/wiki/Cold_start) problem: systems cannot draw any useful inferences about a user before gathering some information about them. This often leads to a vicious cycle: it is hard to gather useful data about users without engaging them, and it is hard to engage them without having some data to personalise their experience. The booking.com researchers argue that, in the travel domain, users are in a _continuously_ ‘cold’ state as their needs change over time, as any data they do have (say, on that backpacking trip you took last year) may no longer be relevant to your current needs (the upcoming family-friendly trip with your children).

### Opportunities and Solutions
The gist of the solution outlined in the booking.com paper is about clustering those users that we _do_ have troves of data about, in order to create a set of stereotypical users (formally: cluster centroids). They use data from their [destination finder](http://www.booking.com/destinationfinder.en-gb.html) (which asks: what is your passion? Where do you want to go?) to create these groups — even going so far as to include users’ device type when creating clusters. More broadly, each time a user engages with their system, they create a _contextual user profile_, which includes where a user is, what they look for, and what time of day it is. That way, as soon as a new (cold) user performs _any action_, the system can quickly identify what ‘stereotype’ (cluster) they most likely approximate — and start recommending off the back of that. Although their evaluation is somewhat under reported, they claimed that using this lead to a 20%+ rise in engagement. The main criticism that was discussed, however, was that this work was fairly (and, arguably, unnecessarily) jargon-heavy: the argument that is made about _continuous_ cold start could be made in other domains as well.

What the booking.com researchers discuss is just one of the _many_ solutions that have already been explored to address the cold start problem. In general, they all rely on either obtaining new data quickly, or paving a path towards personalised results by starting with non-personalised recommendations. The approach discussed in this paper, however, highlights the potential of using your company’s _unique_ (and likely implicit) _data_ to tackle cold-start. This data, alongside approaches that leverage _instant personalisation_, could quickly make use of any signal we get from users to offer them engaging and interesting experiences.

### Conclusion
If you are interested in the technical side of this work, check out [the paper](https://arxiv.org/abs/1607.07904), check out our current [job openings](http://www.skyscanner.net/jobs) at Skyscanner, or get in touch on [twitter](https://twitter.com/neal_lathia).
