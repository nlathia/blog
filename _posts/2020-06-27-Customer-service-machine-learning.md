---
layout: post
title: Customer service is full of machine learning problems
description: And it's not all about bots!
categories: [machine-learning, monzo]
---

<meta charset="utf-8">
<title>Redirecting to https://nlathia.github.io</title>
<meta http-equiv="refresh" content="0; URL=http://nlathia.github.io/2020/06/Customer-service-machine-learning.html">
<link rel="canonical" href="http://nlathia.github.io/2020/06/Customer-service-machine-learning.html">

For many folks, the words "customer service" do not bring to mind images of a high-tech problem space - it certainly was not the case for me. The only public discourse that I'd read about customer service and tech is about how bots were promised to replace human agents, but have mostly been disappointing - and limited to giving poor answers to simple questions. 

After working closely with a bunch of customer service squads at Monzo, I've changed my perspective on this. I believe that building systems that enable and optimise customer service operations is going to become an entire industry soon - specifically, one that can be very profitable for companies that apply NLP and machine learning in their offerings to tackle the specific problems that prevent customer service functions from scaling.

What are these problems? I've written this post - echoing an internal document that I shared inside of Monzo over a year ago - to describe how much more there is to it than just bots. This post is broadly a list of ideas; it includes some things that my team has built and other ideas that have yet to be prioritised.

### ℹ️ Customer service as information asymmetry

To get us started and to give a little bit of background, here is a framing of the broad problem that customer service is trying to solve that I pitched.

Customer service can be thought of as a problem of information asymmetry: customers have questions and companies have answers. Questions and answers can be broken down further:

- There are **two types of questions**: questions seeking information ("do you do refunds?") and questions seeking an action ("may I have a refund?").
- There are **two types of answers**: answers that companies may be happy for customers to find by themselves ("here is our refund policy"), and the answers that they may want to control manually ("yes, I've initiated the refund").

This creates a delicate balancing act for a business: providing excellent service is known to boost brand loyalty and makes customers happy, but maintaining an organisation where agents can reply instantly to everyone is inefficient and infeasibly expensive. 

Machine learning systems have a great history of helping with problems that boil down to information relevance and discovery. With this lens, customer service can be broken down into two non-mutually exclusive systems: (1) systems that enable self-service and (2) systems that enable and support agent productivity.

### 🔍 What problems could machine learning help with?

With that background, let's dive into the list. For each area, I've covered the _why?_ (the business problem), the _how_ (the ML problem formuation), and the _so what?_ (how having this type of system helps).

### 🕵️‍♀️ Helping customers discover answers

**Why?** Many companies manage a set of frequently asked questions (FAQ) pages for customers; these are broadly useless without having a way for customers to discover answers in them.

**How?** Search is one of the most classic machine learning problems. In the customer service context, it translates to:

> How easily can customers find the right FAQ content?

This can either be framed as a **ranking problem** (sort FAQ content from most to least relevant), a **recommendation** problem (suggest articles that are relevant to what the custommer is currently doing) or as a **question-answering problem** (find the content within FAQ pages that directly answers a question).

**So what?** The best possible outcome - for the customer and the company - is for customers to not need to get in touch, because the information they need is readily available and discoverable.

### 🏦 Helping companies categorise known topics

**Why?** From a macro-level, companies can use an understanding of what customers are talking about to decide how to react as the landscape of queries changes.

**How?** Many companies tackle this by asking agents to manually categorise conversations, by using tags: *topic categorisation* is the ML equivalent:

> Given a new query from a customer, what is the customer asking about?

This is a **classification problem:** given a set of N topics (that have typically been defined by the business), we need a system that can accurately recognise whether a new query is about one of those topics.

Categorisation can go beyond **query topics;** there are a whole host of non-topic based categorisations (e.g., urgency, sentiment, language detection) that could help.

**So what?** Knowing what a customer is chatting about enables a host of analytics, reporting, and automation. If anything, having an agent _confirm_ a recommended tag rather than tag manually may save a lot of time!

### 🏦 Helping companies discover unknown topics

**Why?** When you build a product, there are likely to be many things you expect customers to ask you about (e.g., refunds, orders, faulty items, etc.). However, there are also a lot of unexpected topics. Discovering these early could enable the business to jump onto problems before they impact all customers.

**How?** The main problem that *topic discovery* could try to solve is:

> What new (or unusual) topics are customers talking to us about?

This is a **unsupervised learning** problem: given a set of conversations that have occurred recently, can we discover new topics that are coming in at high volume that the company does not have a process to support?

**So what?** One thing that wasn't intuitive for me is how changes in customer service queries could have nothing to do with your business (or global affairs). For example, when [Ticketmaster's systems were breached](https://monzo.com/blog/2018/06/28/ticketmaster-breach), there was a rise in customer support queries at Monzo.

### 👮‍♀️ Helping agents get up to speed quickly

**Why?** When agents pick up a new support ticket, the first thing they probably need to do is to understand what is going on - what the customer is asking, who the customer is, what (if anything) has already happened or been done, and so on.

**How?** The main problem that *case summarisation* could try to solve is:

> What is this relevant information about this case?

For long conversations, this could be treated as a **text summarisation** problem; it could also be framed as a **question extraction** problem. Thinking about how to surface the customer information (what's the status on which order? what segment of customer?) that is relevant to resolving the question at hand could be treated separately.

**So what?** Many customer service experiences today are frustrating because the only way that agents have to get this information is to ask the customer (sometimes, more than once). By having ways for agents to see the most relevant information about a customer, they can get to work faster.

### 👮‍♀️ Helping agents say & do the right thing

**Why?** Companies will have policies and procedures that dictate how different cases should be handled - for example, refunding a customer may be a multi-step process. Many will also have responses with pre-formatted replies to specific types of questions. In some domains, there is a very limited set of actions that an agent could do (e.g., issue a refund). In others (*cough* banking *cough*), this range of actions can quickly become huge.

**How?** The main problem that task-content matching is trying to solve is:

> Given a query from a customer, what content do we have that is similar?

This is a **ranking problem**: given a set of N pieces of content (e.g., stored responses or procedures), we need to build a system that can sort that content from most to least relevant, given the customer query.

**So what?** Handling a customer query swiftly and effectively is the epitomy of good service: if agents need to spend majority of their time trawling through manuals and typing out every single word of a reply, their responses can be frustratingly slow.

### 👮‍♀️ Helping agents to be productive

This is one of the ideas that my team worked on - with mixed success - and I will be blogging about it separately.

**Why?** Context switching is a well-known productivity killer for engineers and data scientists. With customer support agents, it's no different: if every single query they attend to is completely different from the last, they are likely to spend majority of their time context switching!

**How?** The main problem that *task matching* is trying to solve is:

> When an agent needs a new task, which of the available ones should be assigned?

This is a **ranking problem**: given a set of N unassigned tasks that are waiting for an agent, and an agent (who may have specific skills and permissions) who needs a new task, a system that sorts the available tasks from "best match" to "worst match" can help to maintain the agent's productivity throughout the day.

In this case, "best match" could be framed in many different ways, such as *similar to tasks that the agent has successfully resolved recently.* 

**So what?** The traditional formulation of support systems is that customers create or submit "tickets" which need to be "routed" to agents. Switching to a ranking mindset can spare the organisation from building and maintaining specialist teams that work needs to be "sent" to.

### 👮‍♀️👮‍♀️ Spreading knowledge across agents

**Why?** Expert knowledge in customer service is typically manually curated (in processes and knowledge management systems) or inbuilt into the company hierarchy (which manifests as "I'm going to escalate your query to a specialist team/to my manager"). However, the questions that customers ask are rarely unique: it is very likely that a similar query from another customer has been resolved before.

**How?** The main problem that *case similarity* could try to solve is:

> Given a chat case, what (resolved) cases are the most similar?

This is a **ranking problem:** given a case that has just been assigned to an agent, sort the resolved cases (perhaps with some conditions, e.g.: resolved quickly and with a high customer satisfaction) from "best match" to "worst match" and show the agent the top-3 most similar cases.

**So what?** I believe that this approach could be useful to help agents see how other cases have been solved by others — potentially increasing knowledge discovery and reducing the need to escalate things to other teams.

### 🏦 Monitoring outcomes as they happen

**Why?** There are two common ways that companies track the quality of their customer support: by asking customers (with a customer satisfaction survey) or by reviewing cases (using a quality assurance team). Both of these are lagging metrics that only happen after-the-fact.

**How?** The main problem that *case monitoring* could try to solve is:

> Given a case that is ongoing, what outcome is likely to happen?

This could cover a variety of **regression problems:** given a case, we could try to predict how long it will take to handle, what the likelihood of complaint is, what the customer's satisfaction rating will be, etc.

**So what?** This approach could be useful if we wanted to intervene in specific cases (e.g., detect that a conversation is likely to end up in a complaint so that we can do something about it earlier).

## 📜 The end!

This list of ideas switches customer service from "this will be replaced in its entirety by bots" towards "automation can help human service scale." 
