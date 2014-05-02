---
layout: post
categories: 
- digitalian
- notes
comments: true
title: Spaced Repetition with Neo4J
image:
  feature: birch-trees.jpg
  credit: Francesco Gallarotti
  creditlink: http://www.gallarotti.net
---
[Spaced repetition](http://en.wikipedia.org/wiki/Spaced_repetition) is a learning technique that incorporates increasing intervals of time between subsequent review of previously learned material in order to exploit the psychological spacing effect, the phenomenon whereby animals (including humans) more easily remember or learn items when they are studied a few times spaced over a long time span ("spaced presentation") rather than repeatedly studied in a short span of time ("massed presentation").

Following [Lietner System](http://en.wikipedia.org/wiki/Leitner_system), we are going to use a fixed number of *learning boxes* - in our specific case we will use four boxes. All **cards** will initially be placed in the first **box**. When a card is presented to the student for recognition, there are two possible outcomes:  

- if the card is correctly recognized, it will be moved to the next box
- if the user fails to recognize the card, the card will be moved into the first box

Extending this method to improve long-term memory, once a card reaches the last box and it is correctly recognized, it will also be moved back into the first box for another round of learning (achieving therefore level two of learning). All cards must go through three levels of learning. At any point in time, a failure will bring back the failed card to the first box, resetting it to level one as well.

This method can be summarized in the following two graphs. Each **box** has **3 levels** of learning (L1, L2 and L3). All cards start from Box 1 / Level 1 and start moving following the **next** relationships shown in the first graph. .

![Next Level Relationships](/assets/2014/05/LevelsNext.png)

A failure moves the card along the **back** relationship shown in the second graph. 

![Back Relationships](/assets/2014/05/LevelsBack.png)

These graphs essentially embed most of the business logic into nodes and relationships.

Next, let's take a look at the relationships between the *student**, the **cards** that he is learning, and the **boxes**. Because we want to minimize repetition of data, each card will be a single node in the graph database and will be share across all students. Furthermore, there will be one single set of boxes (and levels), shared by all students as well. 

This means that we need to find a way to represent *which cards are in which box at which learning level for which student*. To hold this piece of information, we introduce the concept of **unit of work** which connects a **student** with the **cards** that he is currently learning, while keeping track of the **level of learning** achieved so far.

![Back Relationships](/assets/2014/05/UnitOfStudy.png)





