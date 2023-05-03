---
title: Subordinate and Coordinate
draft: True
---

## Subordinate and Coordinate Arguments

In more complex arguments, there can be multiple premise arguments directly supporting or opposing the same claim, each initiating a separate argument thread. And these threads can branch at any point.

The subtree comprising all the argument threads under a claim is called a **coordinate argument tree**.

Note that arguments for or against the *premises* of the arguments in a coordinate tree are not part of the tree. These arguments are *subordinate* to the arguments in the tree, and therefore belong to separate coordinate trees.

The terms "subordinate" and "coordinate" were chosen by analogy to the concepts of subordinate and coordinate clauses in grammar. They neatly convey the idea of a set of things that are below one thing while being at the same level with each other. And not coincidentally a list of subordinating and coordinating conjunctions in English seems to suggest their use in argument. Subordinating conjunctions seem to initiate argument threads by suggesting reasons or causes: "because", "since", etc. Coordinating conjunctions seem to suggest either adding a parallel (but coordinate) argument thread ("and"), or continuing with the next warrant argument in a thread ("but"). 

The chart below illustrates two separate coordinate argument trees. 


<img src="/assets/images/distributed-bayesian-reasoning/coordinate-argument-trees.svg"
     alt="Coordiante Argument Trees"
     style="display: block; margin-left: auto; margin-right: auto; width: 700px" />

Note that all the arguments in the same coordinate tree have the same color: as in the previous charts, arguments are shown in the same color as the claim or argument they support or oppose. It should be clear now from the definition of a coordinate tree that all arguments in the tree must therefore inherit the color of the root claim. Note also that the argument identifiers in a tree all start with the root claim.

The definition of coordinate argument trees is significant for the mathematical calculations behind distributed Bayesian reasoning, because the method requires estimating how the **combination of all the premises in the coordinate tree** effect the probability of acceptance of the root claim. Mathematically, we cannot make conditional independence assumptions about combinations of premises in the coordinate tree, for the same reason we cannot make conditional independence assumptions about the premises in single argument thread. 

We can however make conditional independence assumptions about the arguments in separate coordinate trees. These assumptions make it possible to apply the distributed Bayesian reasoning process across arbitrarily large arguments graphs.