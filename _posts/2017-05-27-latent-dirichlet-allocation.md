---
layout: post
title: "Latent Dirichlet Allocation"
date: 2017-05-27
excerpt: "In which a blind man tries to explain LDA to fellow blind men."
tags: [assignment, artificial intelligence, natural language processing]
feature: /assets/img/2017-05-27-latent-dirichlet-allocation/book-in-library.jpg
comments: true
encoding: utf-8
---

This article is remade with HTML5/CSS3 instead of Markdown [here](/html-content/2017-05-27-latent-dirichlet-allocation/index.html).

-----

Suppose you have the following set of sentences:

1.	I *eat fish* and *vegetables*.
2.	**Fish** are **pets**.
3.	My **kitten** *eats fish*.

**Latent Dirichlet Allocation** (**LDA**) is a technique that automatically discovers what **topics** these sentences are in. For example, given this set of sentences and asked for two topics, LDA might give something like this.
*	Sentence 1: 100% topic A
*	Sentence 2:	100% topic B
*	Sentence 3: 66.7% topic A, 33.3% topic B
*	Topic A: 40% eat, 40% fish, 20% vegetables
*	Topic B: 33.3% fish, 33.3% pet, 33.3% kitten

Of course, the question is how can LDA find out which sentence is about which topic and what is the topic?

#	An Analogy Before Getting To Details
Imagine you've just graduated high school and has just settled in on your university in a different city. You're a bibliophile and an otaku, so you want to know where other bibliophiles might devour their books and where fellow anime geeks might hold their shipping wars. Of course, as an introvert, you can't just *ask* some stranger where your comrade-in-arms might be. What to do?

You made a game plan. You scope out a bunch of different places across town and noting the people hanging out in each of the places (e.g. Orson hangs out regularly at the cafe and at the mall, Rin hangs out at the cinema and the park, etc.). An important thing to remember is that you neither know what kind of people hangs out in where, nor know what each person is interested in.

So, you pick some number K categories to learn (as in you want to learn the K most important categories people can be categorized into), and start by guessing why which person is in which place. For example, you first guess that Orson is at the cafe with people who like X; when you see him at the mall, he's with his friends who like Y; when you see Rin in the cinema, you randomly guess that the Z people in this city are the local cinephiles; and so on.

Of course, random guesses being random, you're probably completely off the mark. To correct this, you:
*	Pick a place and a person (e.g. Orson at the mall)
*	Why is Orson likely to be at the cafe? Maybe because other people at the cafe with whom he shares interest with sent him a message telliing him to come and hang out.
*	In other words, the more people with interests in X there are at the cafe and the stronger Orson is associated with interest X (at all the other places she goes to), the higher the chances that Orson is at the cafe because of interest X.
*	Then, make a new guess as to why Orson is at the cafe, choosing an interest which you think is most probable.

You go through each place and person over and over and over again and your guesses keep getting better and better. If you notice that lots of bibliophiles hang out at the bookstore, and you think Orson is a bibliophile himself, then it's not that much of a leap to guess that Orson is at the bookstore because his bibliophile friends are there. Now that you have a better idea of why Orson is at the bookstore, you can use this info to improve your guesses as to why everyone else is where they are. Eventually, you can stop updating. Take a snapshot of your guesses, and use it to get the information you want:
*	For each category, you can count the people assigned to that category to find out who has interest in what. By looking at the people themselves, you can guess the category as well (e.g., if category X consists of young women wearing Prada shoes and carrying Gucci purses, you might interpret X as the "rich girls" group).
*	For each place P and category C, you can calculate the proportions of people at P because of C, and these give you a representation of P. For example, you might find out that the people who regularly hang out at the bookstore contains 30% geeks, 20% bibliophiles, 20% otaku, 5% jocks, and 25% university students.

{% capture images %}
    /assets/img/2017-05-27-latent-dirichlet-allocation/bookstore-people.jpg
{% endcapture %}
{% include gallery images=images caption="People hanging out at a bookstore consists of multiple groups with multiple interests" cols=1 %}

#	LDA Model
In the above analogy, establishments are **documents**, people are **words**, and interest groups are **topics**. So, just like you scope out people in various establishments to find out what interest groups they are in and what interest group hangs out where, LDA scans documents to find word related to topics, then guess what the document is about and what words the topic contains.

In more detail, LDA represents documents as mixtures of topics that give out words with certain probabilities. It assumes that documents are written in the following fashion: when writing each document, you
*	Decide on the number of words \\(N\\) the document will have (say, according to a [Poisson distribution](http://stattrek.com/probability-distributions/poisson.aspx)).
*	Choose a topic mixture for the document (according to a [Dirichlet distribution](https://www.quora.com/What-is-an-intuitive-explanation-of-the-Dirichlet-distribution) over a fixed set of \\(K\\) topics). For example, assuming that we have two food and pets topic as in the first example, you might want the document to consist of \\(\frac{1}{3}\\) food and \\(\frac{2}{3}\\) pets.
*	Generate each word in the document by
	*	Pick a topic (according to the [multinomial distribution](https://onlinecourses.science.psu.edu/stat504/node/40) you sampled previously). For example, you might pick the food topic with \\(\frac{1}{3}\\) probability and the pets topic with \\(\frac{2}{3}\\) probability.
	*	Use the topic to generate the word itself (according to the topic's multinomial distribution). For instance, the food topic might spit out the word "eat" with 40% probability, fish with 40% probability, and so on.
	
Assuming this generative model for a collection of documents, LDA then tries to backtrack from the documents to find a set of topics that are likely to have generated the collection.

## Example
According to the process above, when generating a document \\(D\\), you might
*	Decide that \\(D\\) will be \\(\frac{1}{2}\\) about food and \\(\frac{2}{3}\\) about pets.
*	Decide that the number of words \\(N\\) in \\(D\\) is 5.
*	Pick the first word to come from the food topic, which then gives you the word "asparagus".
*	Pick the second word to come from the pets topic, which then gives you the word "iguana".
*	Pick the third word to come from the pets topic, which then gives you the word "backyard".
*	Pick the fourth word to come from the food topic, which then gives you the word "Michelin".
*	Pick the fifth word to come from the food topic, which then gives you the word "devour".

The document generated with LDA will be the string of nonsense words

> asparagus iguana backyard Michelin devour

Note that LDA is a bag-of-words model.

#	Learning

The above is just a generating model. Suppose you want to use LDA to learn the topic representation of each document and the words associated with each topic. One way to do this (known as [collapsed Gibbs sampling](https://en.wikipedia.org/wiki/Gibbs_sampling)) is done in three steps.

To illustrate these steps, suppose you have two documents with the following words:

|   | Document X |   | Document Y |
| --- | --- | --- | --- |
|   | Fish |   | Fish |
|   | Fish |   | Fish |
|   | Eat |   | Milk |
|   | Eat |   | Kitten |
|   | Vegetables |   | Kitten |
 
## Step 1: You tell the algorithm how many topics you think there are.
You can either use an informed estimate (e.g. results from a previous analysis), or trial-and-error. In trying different numbers, pick the one that generates topics that makes your documents most interpretable. In this case, the number of topics \\(K\\) can be guessed just by eyeballing the documents.

## Step 2: The algorithm randomly assigns each word in everey document to one of the \\(K\\) topics.
Temporary topics are assigned to each word semi-randomly (according to a Dirichlet distribution, to be precise). This also means that if a word appears twice, each word may be assigned to different topics. In analyzing actual documents, function words (e.g. "the", "and", "your") are ignored.

These random assignment already gives you both topic representations of all the documents and word distributions of all the topics, but they're most certainly very off the mark.

## Step 3: The algorithm checks and updates topic assignments.
The algorithm loops through each word in each document. For each word, its topic assignment is updated based on two criteria:
*	**How prevalent are the topics in the document?**
	*	Count \\(P(\text{topic t} \| \text{document d})=\\) the proportion of word in document \\(d\\) that are currently assigned to topic \\(t\\).
*	**How prevalent is the word amongst topics?**
	*	Count \\(P(\text{word w} \| \text{topic t})=\\) the proportion of assignments to topic \\(t\\) over all documents that come from word \\(w\\).

In this step, we're taking the assumption that all topic assignments except for the current word examined are correct, and then updating the assignment of the current word using our document generation model.
	
As an example, suppose we are now checking the topic assignment for the word "fish" in Document Y:

|   | Document X |   | Document Y |
| --- | --- | --- | --- |
| F | Fish | *?* | *Fish* |
| F | Fish | F | Fish |
| F | Eat | F | Milk |
| F | Eat | P | Kitten |
| F | Vegetables | P | Kitten |

### How prevalent are the topics in the documents?
Since the words in Document Y are assigned to Topic F and Topic P in a 50-50 ratio, the remaining "fish" word is equally likely to be about either topic.

|   | Document X |   | Document Y |
| --- | --- | --- | --- |
| F | Fish | *?* | *Fish* |
| F | Fish | **F** | **Fish** |
| F | Eat | **F** | **Milk** |
| F | Eat | **P** | **Kitten** |
| F | Vegetables | **P** | **Kitten** |

\\[P(\text{topic F} \| \text{document Y})=\frac{2}{4}\\]

\\[P(\text{topic P} \| \text{document Y})=\frac{2}{4}\\]

### How prevalent is that word across topics?
Since the words "fish" in both documents compose almost half of remaining topic F words but none of the remaining topic P words, a "fish" word randomly picked would likely be about Topic F.

|   | Document X |   | Document Y |
| --- | --- | --- | --- |
| **F** | **Fish** | *?* | *Fish* |
| **F** | **Fish** | **F** | **Fish** |
| F | Eat | F | Milk |
| F | Eat | P | Kitten |
| F | Vegetables | P | Kitten |

\\[P(\text{"fish"} \| \text{topic F})=\frac{3}{7}\\]

\\[P(\text{"fish"} \| \text{topic P})=0\\]

### Updating topic assignments
Reassign \\(w\\) with a new topic, where you choose topic \\(t\\) with probability \\(P(\text{topic t} \| \text{document d}) \times P(\text{word w} \| \text{topic t})\\). According to our generative model, this is the probability that topic \\(t\\) generates by word \\(w\\).

\\[P(\text{topic F} \| \text{document Y}) \times P(\text{"fish"} \| \text{topic F})=\frac{2}{4} \times \frac{3}{7} = \frac{3}{14}\\]

\\[P(\text{topic P} \| \text{document Y}) \times P(\text{"fish"} \| \text{topic P})=\frac{2}{4} \times 0 = 0\\]

Based on the probabilities, we'd assign the word "fish" in Document Y to Topic F.

|   | Document X |   | Document Y |
| --- | --- | --- | --- |
| F | Fish | *F* | *Fish* |
| F | Fish | F | Fish |
| F | Eat | F | Milk |
| F | Eat | P | Kitten |
| F | Vegetables | P | Kitten |

After repeating Step 3 many, many times, you'll at one point reach a roughly steady state where your assignments are starting to become somewhat coherent. Use these assignments to estimate the topic mixture of each document (by counting the proportion of words assigned to each topic to the wordcount of the entire document) and the words associated to each topic (by counting the proportion of how many times a word occur to how many words are in the topic).

# Real World Example
If you want to see a real world application of LDA, [Edwin Chen applied LDA to a set of Sarah Palin's emails](http://blog.echen.me/2011/06/27/topic-modeling-the-sarah-palin-emails/).

-----
References, in no particular order
*	[This very enlightening post](http://blog.echen.me/2011/08/22/introduction-to-latent-dirichlet-allocation/) by Edwin Chen, which also teaches me what LDA is.
*	[This post](https://algobeans.com/2015/06/21/laymans-explanation-of-topic-modeling-with-lda-2/) by AlgoBeans, also about LDA for dummies.
*	[This post](https://confusedlanguagetech.blogspot.co.id/2012/07/jordan-boyd-graber-and-philip-resnik.html) by Naomi P. Saphra, which is yet another LDA explanation for a slightly more informed audience.
*	[This post](https://sumidiot.wordpress.com/2012/06/13/lda-from-scratch/) by Sumidot, which gets a bit more technical with formulas and graphs and all.
*	[This Quora question](https://www.quora.com/What-is-a-good-explanation-of-Latent-Dirichlet-Allocation) on simple explanations about LDA.
*	[This awesome post](http://www.matthewjockers.net/2011/09/29/the-lda-buffet-is-now-open-or-latent-dirichlet-allocation-for-english-majors/) by Matthew Jockers explaining LDA to English majors, with the helps of Melville, Austen, and Hemingway. 
*	[This slide](http://cs.brown.edu/courses/csci2950-p/spring2010/lectures/2010-03-03_santhanam.pdf) neatly summarizing [the original paper](https://www.seas.harvard.edu/courses/cs281/papers/blei-ng-jordan-2003.pdf).