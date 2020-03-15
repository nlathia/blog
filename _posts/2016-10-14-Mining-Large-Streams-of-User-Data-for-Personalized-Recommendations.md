---
layout: post
title:  "Mining large streams of user data for personalized recommendations"
tags: [recsys, paper]
excerpt_separator: <!--more-->
---

Lessons from Investing $1M in Machine Learning for Recommendation.
<!--more-->

Data Scientists across Skyscanner have started meeting every fortnight to discuss research papers that tackle similar problems to those that we face within Skyscanner. The [2nd paper](http://www.kdd.org/exploration_files/V14-02-05-Amatriain.pdf) we read was: “Mining Large Streams of User Data for Personalized Recommendations” (hi Xavier!). Just like the last post, we’re we’re also writing up a brief, non-technical overview the problems/opportunities we discussed.

### Lessons from a Million Dollar Investment in Personalisation
Netflix famously announced a [$1M prize in 2006](http://netflixprize.com/), calling on researchers across the world to improve their movie recommender system by 10%. To create this competition, they had to make a critical decision: _how could Netflix measure a 10% improvement in their system_? At the time, most recommendation systems worked by predicting how many stars you would give each item (in Netflix’s case, each film): if they could predict that you would give a film 5 stars, then it would be recommended to you. The company decided that the competition would focus on making those predictions more accurate, by measuring the difference between the prediction and the actual user rating — they used [Root Mean Squared Error](https://en.wikipedia.org/wiki/Root-mean-square_deviation) (RMSE) metric.

Just around the time when researchers started to question whether it was statistically possible to win the competition (e.g., this [paper](http://xavier.amatriain.net/pubs/umap09.pdf)), the prize was won by a team that combined _over_ 100 different algorithms and over 2000 hours of work to create a blend (or [ensemble](https://en.wikipedia.org/wiki/Ensemble_learning)) of predictions that were fantastically accurate.

Many technical innovations came from the competition: matrix factorization algorithms became the state-of-the-art in recommendation systems, neural networks known as [Restricted Boltzmann Machines](https://en.wikipedia.org/wiki/Restricted_Boltzmann_machine) starting coming into vogue (pre-dating the current “deep learning” hype), and important temporal qualities like _when_ a user rated a film were found to be highly influential in determining _how_ they rated it.

The most surprising result, however, was how little of the thousands of hours of work that went into this competition actually ended up affecting Netflix’s production systems. In particular:

**$1M in research is not $1M in production**. Netflix only implemented a handful of the hundreds of algorithms that formed the winning solution. A huge amount of engineering work had to be dedicated to productionise them: scaling the competition algorithms to handle billions of ratings rather than the competition’s 100M and making the system adaptable to new ratings rather than using the static dataset they had provided were two of the main challenges discussed.

**Netflix had ‘moved on’ from the proxy question they asked**. As the business moved towards its online streaming model, it turned out that customer ratings (explicit data) were not the best data to source for recommendations: they now had tons more implicit data from their customers, by just logging how people were using the system (clicks, searches, views, etc.). Predicting a rating accurately was no longer as important as ranking films in the right way with other sources of data.

**Netflix had started to optimise for user experience metrics**: like engagement, freshness, trust, retention. They came up with a feedback loop of forming hypotheses, training offline models and experiment with them online (via A/B tests). They were able to iterate fast,reject/accept their hypotheses and reason about the results for hundreds of features. A bunch of their recent research papers demonstrate this.

The final point contains the real $1M question: how can data scientists work (offline) on an algorithm and measure improvements that will correlate with improvements in online experiments? It’s hard but an answer to this could speed up a lot of experimentation!

![about](/images/posts/NetflixPrizePaper.png)

### Key Takeaways
In the current Netflix system, everything that you see has been, to some extent, tailored to “your” tastes (Netflix deals with households of customers, so they tailor to a group at a time). This includes the rows of films you see, the ordering of the rows, and the ordering of the films inside each row.

In most web services, implicit and binary feedback is much more readily available and requires no extra effort on the user side. Many algorithms have now been developed to ingest this data to build useful services.

The algorithm is only one piece of the puzzle. The metric (or multiple metrics) you are optimising for, the algorithm under the hood, and the interaction that users have with recommendations can all equally impact the business. They even mention awareness: users should know how well the service is adapting to their tastes, by providing explanations. How do we explain our algorithms to our users?

Are you aware of your users’ context? Even in something as ‘simple’ as recommending a film, location, time, and other contextual variables can have a very large impact.

Can you explain your algorithms’ effect on our experiments? Data features have an added layer of complexity when it comes to experimentation, as the number of variables increases a lot: offline experiments, model parameters, online experiments, UI biases. We should be able to have better answers (even approximate) than yes/no— we should be able to answer why’s, and be able to optimise and adapt while online.

### Conclusion
If you are interested in the technical side of this work, check out the paper, or get in touch on [twitter](https://twitter.com/neal_lathia), — more reading group summaries are on the way — I’m already behind on these blog posts.
