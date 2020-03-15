---
layout: post
title: Neural approaches to conversational AI
description: This is a a six chapter review! It alternates between giving overview-level information for specific areas of conversational AI, and diving into details of deep or reinforcement-based learning algorithms as applied to conversational settings.
categories: [paper]
---

Available: [https://arxiv.org/pdf/1809.08267.pdf](https://arxiv.org/pdf/1809.08267.pdf)

The stand-out points (for me) were about problem formulation (e.g., single vs multi turn dialog) and that nearly all sections discussed that robust evaluation is a core problem in dialog systems: we don't have generalised, automated, scalable, robust ways to determine that a conversation with a computer has gone well.

## Chapter 1. Introduction

Intelligent dialogue systems are on the rise thanks to breakthroughs in deep and reinforcement learning. The research field is growing too, attracting folk from the NLP, IR, and ML communities. A number of tutorials and surveys have been published - this chapter is another survey, based on tutorials given at SIGIR and ACL 2018 - that hopes to provide a unified view of conversational / dialog systems.

The kinds of problems that dialog systems are expected to solve include:

* **Question answering**: providing a concise, direct answer to user queries based on rich knowledge drawn from various data sources.
* **Task completion**: accomplishing a task for the user (e.g., restaurant reservation, business trip planning).
* **Social chat**: conversing seamlessly with the user and providing useful recommendations.

Bots can be grouped into two categories: "task oriented" and "chitchat." All of the personal assistants on the market today (e.g., Alexa) are task oriented and handle simple, one off tasks (e.g., reporting the weather). They are typically composed of four modules: (1) Natural Language Understanding, (2) state tracking, (3) dialog policy, (4) Natural Language Generation -- although there is now a trend to develop all four of these together using a neural network. Most of these will have access to external databases that supporting completing the task.

An example of a chat-oriented bot is XiaoIce - there's an overview of it in the recent [Import AI newsletter](https://jack-clark.net/2018/12/25/import-ai-126-what-makes-microsofts-biggest-chatbot-work-europe-tries-to-craft-ai-ethics-and-why-you-should-take-ai-risk-seriously/). Their primary goal is often to be "companions to humans with an emotional connection." 

The unified view casts dialog as an *optimal (hierarchical) decision making* process. The top level is about picking the right agent (e.g., question answering vs. meeting scheduling), the lower level is about picking the right action. These can be thought of as options over a Markov Decision Process, which fits well with reinforcement learning.

A common metric to use as a reward function is CPS: **Conversation-turns Per Session**. There are two views on this metric: it sometimes needs to be minimised (complete a task quickly) and other times needs to be maximised (engage in long/meaningful conversations). The can be combined by reasoning over multiple sessions: being able to complete a task quickly (small short-term CPS) means that users will find the dialog system useful and will use it often (large long-term CPS).

Neural-based NLP tasks are usually performed in three steps: (1) encoding user input (words) into vectors, (2) reasoning in the vector space to generate a response, (3) decoding the response into a symbolic space (back to words). End-to-end training of neural nets results in a tighter coupling between the architecture and the application -- the focus shifts away from having the right components for language processing (e.g., parsing, context reasoning, etc.) towards having the right architecture.

## Chapter 2. Machine Learning Background

This chapter is an overview of ML. I'm only going to write notes about bits that are specific to conversational systems.

Widely used neural network layers for text classification:

* Embedding layers: mapping each word to an m-dimensional vector.
* Fully connected layers: performing linear projections
* Convolutional-pooling layers: forms a local feature vector of a word and its adjacent words (within a fixed sized window).
* Recurrent layers: map the text to a dense/low dimensional vector by sequentially and recurrently processing each word.

Two examples: [Deep Structured Semantic Model](https://www.microsoft.com/en-us/research/project/dssm/) and [Seq2Seq](https://google.github.io/seq2seq/).


## Chapter 3. Question Answering and Machine Reading Comprehension

Question Answering (QA) allows a user to query a large scale knowledge base/document collection in natural language. They differ from search engines in that they aim to provide a concise and direct answer rather than a ranked list of results. QA is split into single-turn and multi-turn, with the latter being an emerging research topic.

Knowledge base (KB) question answering has been developed based on symbolic approaches -- which has been hard to scale. KBs are sometimes known as knowledge graphs - entities are nodes and relations are directed edges between them. Finding an answer to a question is based on semantic parsing: mapping a question to a "meaningful" representation, finding the paths in the graph that match the query, and then retrieving the nodes. This approach has two challenges:
* Paraphrasing: there are many ways to ask the same question. Embeddings have been a useful way to tackle this.
* Search complexity: number of candidate paths grows exponentially with the path length.

Embeddings map entities and relations to continuous vectors in a (neural) space: this space can be viewed as a hidden semantic space where various expressions with the same semantic meaning map to the same vector. There are various ways (e.g., ReasoNet using DL and DeepPath using RL) to perform multi-step knowledge based reasoning using these vectors. 

For multi-turn QA, users need to interactively compose (complex) queries, where the agent can ask clarifying questions. The Entropy Minimization Dialogue Management strategy picks the next question to ask based on the attribute that has the most entry in the remaining KB entries.

Machine Reading Comprehension is the task of answering questions given a set of text passages. Answers are defined as a span of text in (one of the) passage(s). There are many datasets (e.g., SQuAD) that are fueling research in this area. As with approaches above, this is usually done in 3 steps: encoding, reasoning, and decoding.
* Encoding: Popular pre-trained embeddings include word2vec and GloVe, and can be enhanced with part of speech tags. Contextual embeddings (e.g., ELMo and BERT) uses contextual cues from surrounding words to refine the embeddings.
* Reasoning: can be done in a single-step (match the question and document only once) or multi-step.

Many of these systems are developed as single-turn. To make this conversational, the system also needs to include (encode) the conversation history.

## Chapter 4. Task-oriented Dialogue Systems

Completing a task is different from question answering -- they are typically domain dependent (e.g., book a hotel room). There are a variety of types, including slot-filling dialogue: where the machine needs to collect information from the user in order to complete the task.

The interaction between a dialog agent and a user mirrors the interaction between an RL agent and the environment. An appropriate reward function in this domain would give a high reward for completing the task and a small penalty for each required intermediate turn. In practice, reward functions are often a linear combination of a subset of the following:
* Task completion success
* Time elapsed (number of turns)
* Coherence, diversity, personal style(s)

For RL, researchers have developed user simulators for RL agents to converse with -- since conversing with real users is expensive. The simulators could be based on a user agenda or based on models/data. Ultimately, many evaluations rely on recruited users (small scale) or A/B tests.

Slot filling problems also rely on dialogue state tracking -- containing all information about what the user is looking for at the current turn of the conversation. This is the input information to the dialogue policy which decides what action to take next. For example, see [this challenge](https://www.microsoft.com/en-us/research/event/dialog-state-tracking-challenge/). 

An example dialogue policy is DQN, which takes as input an encoding of the current dialogue state. It outputs a vector who entries correspond to all the possible dialogue-act/slot pairs that can be chosen by the system. Learning the policy from scratch takes a lot of data, but can be sped up by using expert-generated dialogues, imitation learning, or replay buffer spiking (warm start). In online settings, an agent has to balance between exploit (maximise reward) and explore (discover better alternatives) -- exploration when neural nets are used is an active research topic. This is made even more challenging when needing to extend the agent's domain while serving users (adding more intents/slots).

Dialog can often be decomposed into a sequence of related sub-dialogues that each focus on a single topic. For example: booking a flight, hotel and car rental for a single trip. These are referred to as composite-task dialogues, where subtasks may have interdependencies. On the other hand, if a dialog can have more than one broad subject matter (e.g., hotels and restaurants), this is referred to as multi-domain dialogue.

Natural Language Generation (NLG) aims to convert a communication goal into natural language form. Rule and template based approaches are the most common ways to achieve this. Newer approaches (corpus-based) aim to optimise generation from a corpora of text. There are a number of LSTM based approaches that have been explored here.

All of these modules in a dialog system are often optimised separately. If all of them are differentiable, then the whole system can be optimised by backprop. Supervised and reinforcement learning have both been applied to this scenario.

Beyond slot-filling dialogue:
* Information seeking dialog
* Navigation dialogue
* Multimodal dialogue with non-verbal inputs
* Mixed initiative and negotiation
* Multiple-parties

## Chapter 5. Fully Data-Driven Conversation Models and Social Bots

End to end systems do not rely on expert knowledge and do not have the traditional components described in the previous chapter. These are used more often for chit chat scenarios rather than task completion. Many of these systems are inspired by statistical machine translation.

Early systems represented the data as query/response pairs, which limited the context of the responses. Researchers have since explored RNN/LSTM based approaches that exploited a longer context. Other approaches such as the Hierarchical Recurrent Encoder-Decoder (HRED) aim to exploit longer-term context. Attention based models allow for conditioning on parts of the sentence that are relevant for prediction.

Challenges in these systems:
* Response blandness - responses are often bland, deflective, uninformative (e.g. "I don't know"). A variety of methods, including GANs, have been explored to increase the diversity of responses.
* Speaker consistency - responses are incoherent, where the system contradicts itself. This is often a result of a one-to-many of examples in training data (e.g., there are many answers to "how old are you?").
* Word repetitions - since it is not clear how often a specific word or phrase should be repeated in the output.
* Response appropriateness - struggling to produce names and facts that has appropriate semantic content.

Grounded conversation models aim to help to effectively converse about a user's environment, by factoring in additional information (e.g. using images from the environment).

Supervised learning in end to end conversation training is hard because human-human conversation training data is very different from online human-computer scenarios. These approaches also optimise for an immediate reward rather than a long-term one. Reinforcement learning in this domain is hard because of the difficulty of coming up with the right reward function.

Researchers have tried using social media data (e.g., twitter or reddit) to train these systems. They usually need to reconstruct dialogues from the posts, and sometimes run into trouble with API limits. Evaluation is just as challenging as previous sections -- it's common to use human raters, alongside metrics like BLEU, ROUGE, and METEOR. There is research that shows that human ratings and these metrics do not correlate.

## Chapter 6. Conversational AI in Industry

Gives a number of examples, e.g. Bing QA, Siri, Google Assistance, Alexa, Cortana, etc.

Most notably, this section highlights differences between research and application:
* Scale and quality of text
* Latency
* User experience

Many of the actual chatbot examples do not use any AI at all, and fully rely on handcrafted rules.

