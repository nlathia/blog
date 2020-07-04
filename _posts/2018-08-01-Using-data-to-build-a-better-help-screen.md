---
layout: post
title: Using data to build a better help screen
description: At Monzo, we use data to help our customers find their own answers faster. 
categories: [monzo, data-science]
---

Originally published on [monzo.com](https://monzo.com/blog/2018/08/01/data-help/). 

Our customer support team are always on hand if you need to chat with a human, but that isn’t the only way to find answers: the help screen now has over 400 pages that answer common questions, from activating your card to travelling abroad.

As we work to make sure we can keep giving you world-class support as we scale, we’ve been using data to make changes to the help screen so you can find your own answers faster.

### Measuring how helpful we are
If you scroll to the bottom of any page in the help screen, you’ll see a button that says ‘I can’t find what I’m looking for.’ If the page you’re reading didn’t have the right answer, clicking it will put you in touch with someone from our customer support team.

When you do that, it also gives us a powerful signal that the page you were reading wasn’t good enough to answer your question. If you don’t click the button, that’s another useful signal, that maybe you did find all the information that you needed.

We use these signals to prioritise the help pages we need to improve: if lots of people have to start a chat after reading a certain article, we know we need to make it clearer or add some extra information. We’ve set up a dashboard that tracks each page’s performance in terms of ‘conversion to chat,’ which lets us quickly identify pages that we need to improve, and measure any changes after we update a page.

We also measure how people behave when they use the help screen. We call a set of interactions with the help screen a ‘session,’ and each one can include one or more of the following actions: searching, seeing a suggested help page, navigating the categories, and starting a conversation through in-app chat.

From tracking this data, we found:
* Customers who used our search functionality were much less likely to get in touch with customer support.
* If people opened an article but didn’t find what they needed, they were very unlikely to go back and search again. Most people opted to chat to someone in our customer support team straight away.

These insights told us that searching and navigating help pages isn’t as seamless as it should be. So we’ve been using them to make the help screen better!

### Easier navigation with related articles
Many of our help pages are about very similar topics, but each one’s relevant in a slightly different scenario. For example, we have separate articles for “I forgot my PIN,” “My PIN is blocked,” “I don’t know my PIN,” and “I want to change my PIN.” But if you search for “PIN,” we’ll show you most of these results.

In the past, if you clicked on one of these search results but didn’t find the answer you were looking for, you’d need to go back into your search results to browse the other answers, or return to the help screen and start all over again.

Because this process wasn’t easy, people were more likely to tap ‘I can’t find what I’m looking for’ and get in touch with customer support instead.

#### What we built
So, to make it easier to find the right answer after landing on the wrong search result, we’ve added ‘Related articles.’ We now list other pages that are semantically related to the one you’re currently reading.

<img data-width="1600" data-height="1320" src="https://cdn-images-1.medium.com/max/800/0*EA_E54jnSAHFPWq1.png">

To work out whether two pages should be related, we’ve built a system that makes it easy for us to run A/B tests. It means we can show different customers related articles that had been computed in different ways, and work out which method is most successful.

So far, we’ve tried some relatively simple methods:
* Manually categorising articles: We tried relating articles using the categories we set manually when we write content for the help screen. Two articles are related if they also appear in the same categories that you can navigate in the help screen.
* Using machine learning: We also tried relating articles using an open source machine learning model that was trained on millions of words from a Google News dataset.


#### Does it work?
In a short series of experiments, we learned that showing customers related articles significantly lowered conversion to chat across the board: people were able to find the answer they needed, even if they landed on the wrong one at first, without needing to chat with someone in customer support.

Interestingly, using machine learning showed no significant improvement compared to grouping articles manually by category. There are a couple of reasons why this may be the case. First, we invested a lot of time to make sure that pages were grouped together properly. Second, we used an open source machine learning model to compute related articles, rather than using our own data. We think training them on our own data might produce better results.


### Integrating search throughout the help screen
Our analysis of help screen sessions also showed that customers who use search were approximately 20% more likely to find the answer that they needed, without having to get in touch with customer support.

Because search seems to be the most effective way for people to find the answers that they need, we decided to find out how we could help customers benefit from our search results, without forcing them to actually search.


### What we built
We’ve been running some experiments and making changes to the help screen, like putting the search bar at the centre to make searching more prominent.

We also ran an experiment that showed you search results as you typed a message to our customer support team. For example, if your started telling us that you forgot your PIN, we’d suggest a series of relevant articles. Under the hood, these suggestions use the same machine learning service that powers search on the help screen. And we didn’t add any additional friction to the process of getting in touch, so you’d still be able to start a new conversation.

<img data-width="1600" data-height="1320" src="https://cdn-images-1.medium.com/max/800/0*1xYnSQp4-QEGJ9QB.png">


### Does it work?
We found that suggesting relevant articles as you typed helped you find the answer you needed as effectively as searching yourself, so we added the feature to the apps!


## What’s next?

### Understanding what you talk to us about
We’ve recently begun another round of analysis, to find out what customers talk to us about when the help screen hasn’t been helpful. We’ll use this analysis to help us identify areas that the help screen doesn’t cover at all, and understand how we can improve specific help pages even more.

### More nuanced machine learning
We’ve started prototyping various ways that we can use the data we collect to tweak the existing machine learning features on the help screen.

Right now a lot of our machine learning focuses on the content of each page, and doesn’t factor in the implicit feedback that we get as you view and click on articles. The actions you take when you use our search results are powerful indicators of their quality.

For example, imagine if every time a customer searched to find out what to do if they forgot their pin, they always ignored the first two results and picked the third answer on the list. This would tell us that we should promote the third result to the top of the list, so everyone could find it right away.


### Surfacing other kinds of content
Right now our help search algorithm surfaces help articles with text. But in the future, we want it to be able to show you other types of results as well. For example, if your question’s been answered by someone in the community, we’d like to link you to their post.


### Making better suggestions
We’re also looking to experiment with machine learning in other areas altogether. For example, the suggested articles on the help screen are currently computed using a number of rules, and we have a hunch that personalising these results with machine learning could help us provide better suggestions.


### Preempting questions about specific transactions
We’ve noticed that lots of conversations with customer support are about specific transactions, which is why the help screen now shows your last three transactions.

But we’ve also started to explore how we can identify which transactions you’re likely to have questions about, and link them with useful answers.

All week we’ll be sharing an insight into how we’re working to make sure Monzo’s ready to support one billion customers! Join the discussion on the community forum and keep an eye out on the [blog](https://monzo.com/blog/) and [Twitter](https://twitter.com/monzo?lang=en) for updates 👀.
