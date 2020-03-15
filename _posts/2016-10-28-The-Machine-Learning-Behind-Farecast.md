---
layout: post
title: The machine learning behind farecast
description: Predicting the black box of flight prices.
categories: [recsys, paper]
---

A number of years ago, I ran into [this research paper](http://www.isi.edu/integration/papers/etzioni03-kdd.pdf) “_To Buy or Not to Buy: Mining Airfare Data to Minimize Ticket Purchase Price_,” when I was working at UCL on a project that was researching how to help people navigate cities better using data. I had just received a large data set of Oyster card transactions from Transport for London. This paper, which looks at recommending whether to buy an airplane ticket, inspired us to apply a similar concept to help people decide what travel card to buy, with a recommender system that would learn from their travel patterns. That research resulted in [this paper](http://nlathia.github.io/papers/lathia_kdd2011.pdf) “_Mining Mobility Data to Minimise Travellers’ Spending on Public Transport_” and this [BBC news story](http://www.bbc.co.uk/news/uk-england-london-13389363).

I resurfaced the airfare data paper for the Skyscanner reading group — we’ve been meeting to discuss research papers that tackle similar problems to those that we face within Skyscanner. Just like the last post, here’s a brief, non-technical overview of what we discussed.

### Making a Product out of Price Prediction
Flight prices are notoriously subject to change: I’m sure that most of us have looked at a flight, thought about it for a while, and then come back to it only to find that its price had shot up (or, if we’re lucky, down).

As the authors of this paper explain, part of predicting a flight price seems to be about _decoding_ the hidden decision engine that carriers use to set their own prices: perhaps a current route’s price is based on available seats, on demand, on competitor’s prices, on the time of year, or a multitude of other factors. Given that there are so many different reasons why flight prices may change (and we do not necessarily have access to all the data that providers are using to set prices), could we ever reasonably develop any means to predict flight prices?

This scenario is one where machine learning can come to the rescue. Using the only assumption that there will be some _similarity_ between historical and future route prices, the authors create a model based only on data that is readily available to them on the web: the route (e.g., Seattle-Washington), the days until the departure (e.g., 10), and the historical prices. This model tries to answer the question: _given a route, a departure date, and the current price, should I buy the ticket, or should I wait?_

The authors throw a number of well-known machine learning methods at the problem, such as [reinforcement learning](https://en.wikipedia.org/wiki/Reinforcement_learning) and [time series forecasting](https://en.wikipedia.org/wiki/Time_series), and a [combined model](https://en.wikipedia.org/wiki/Ensemble_learning) which (to no surprise), turns out to be the most accurate. The paper, though, does hold back on a number of fronts: it only gives us results for a small set of hand selected routes, only consider up to 21 days before a departure date (which means that 75% of their users would not be able to achieve any cheaper options), and does not consider any long-term effects like seasonality (to be fair it was before the _big data_ era, hence scalability was an issue for them).

Even so, there are a number of key take-aways in this paper:

**Prediction Accuracy < User Utility.** When faced with a problem like “predicting the value of X,” we’d often be naturally inclined to measure how accurately we predict that value. Instead, these authors measure how well such a prediction could support a user action, in this case “wait” or “buy.”

**Predicting is not the same as informing.** The recent announcements from Google show that they have implemented something like this in their [flights search](https://blog.google/topics/travel/making-travel-planning-less-stressful-holiday-season/). However, they _inform_ users based on historical trends (“this fare is likely to increase” rather than “we predict this fare will be x”).

**Time ordered data does not mean a time series problem.** Since the data is ordered by time, one of the ‘natural’ approaches to try was a model that predicts whether a price will go up or down based on its recent behaviour. However, the authors found that flight prices tend to jump — either up or down — and so predicting the next price based on what has happened for the last few hours was one of the worst performing approaches.

The authors of this paper later founded [Farecast](https://moz.com/blog/farecast-an-evolution-in-travel-pricing-prediction), which was eventually [acquired by Microsoft](https://en.wikipedia.org/wiki/MSN_Travel_%28website%29). Bing eventually dropped the Farecast’s prediction feature but other meta-search engines have developed similar products; Kayak have had a price prediction tool for more than 3 years (see their [blog post](https://www.kayak.co.uk/news/kayak-launches-price-forecasts-flights/) announcing the feature), and Google has just launched a prediction tool in their recent [blog post](https://blog.google/topics/travel/making-travel-planning-less-stressful-holiday-season/).

### Conclusion
If you are interested in the technical side of this work, check out the paper, or get in touch on [twitter](https://twitter.com/neal_lathia).
