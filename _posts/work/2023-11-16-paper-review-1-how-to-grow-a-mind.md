---
permalink: /posts/en/paper-review-how-to-grow-a-mind
date: 2023-11-16
title: 'Paper Review: 1. How to grow a mind'
categories:
  - work and research
---

I read this famous [paper](https://www.science.org/doi/full/10.1126/science.1192788) after a reading-group meeting when the host introduced a Theory-of-Mind paper with one author as Noah Goodman. I am personally interested in modeling cognitive functionalities of human brains into language neural networks to enable their efficient learning ability. The below is my summary of the paper with my thoughts.

# How to grow a mind?

Scientist have long questioned about the ability of getting so much from a little experience (i.e. little exposure to a new concept). Perhaps through the learning of new concepts, our brain constructs a large-scale system of knowledge that goes beyond all the observed data. Indeed, another source of information must make-up the difference, which is so-called constraints (w.r.t. psychologists and linguists), inductive bias (w.r.t. AI and ML scientists), and priors (w.r.t. statistician).

The article reviews recent models of human learning and cognitive development through the lens of Bayesian/Probabilistic AI. The key ideas are demonstrated via the answers to the central questions:
1. How does abstract knowledge guide learning and inference from sparse data?
2. What forms does abstract knowledge take, across different domains and tasks?
3. How is abstract knowledge itself acquired?

The reviews also draw contrasts with two earlier approaches to the origins of knowledge: nativism (our perception of concepts is innate, and activated by our exposure to the world) and associativism (we learn and neurally represent concepts via experience/observation).


## The role of abstract knowledge
Many aspects of higher level of cognition has been illuminated by the Bayesian statistics, such as similarity (used in hierarchical clustering and many other clustering methods), representativeness, ... Bayesian analyses can manifest many cognitive capabilities resulting from unconcious thinking (system 1), including perception, language, and memory, however, it does not suffice to represent capabilities associated to explicit concious thinking (rules, or sophisticated knowledge that require deep training).

(The review then provides background of Bayesian probability, and an example. It's pretty easy to understand if you have background in Statistical ML)

## The form of abstract knowledge
It is impossible to simply list all the hypothesis (instance-possible_class in concept learning or instance-instance in causal learning) along with its prior and likelihood. Therefore, more sophisticated form of knowledge representation must be employed for our cognition.

Tree structure is prevalent in Bayesian statistical models (e.g. hierarchical reasoning), however, it's data dependent (see Figure 2 in the paper). Other forms of abstraction such as chain, rings, and their combinations can best fit a certain dataset of interest. Yet, there is a common parent structure of all these forms: directed graph. A graph can have nodes representing concepts and directed egdes representing probabilistic relational link (Note: this ideal aligns with the vision of my supervisor in formulating our-group knowledge base ASER into probabilistic format, like a BeliefGraph introduced in a paper of Nora Kassner). Each of the form of knowledge will shape the graph in different way with different kinds of prior distributions.

## The origins of abstract knowledge
Or in other words, the origin of the form of knowledge in each scenario. How humans know which form is the best structure to organize hypotheses?

The acquisition of abstract knowledge or new inductive constraints is primarily the province of cognitive development that only by (enough) experience, our cognition knows the best forms to organize categories. That is opposed to the nativists' belief that these forms must be innate (in equivalent to conventional ML algorithms that assume a fixed form of structure), and debatable in comparison to connectionists' view that the generic learning system only best approximates tree, causal networks, and other forms of structure that people know explicitly (which does not give room for unseen form of knowledge).

Hierarchical Bayesian models (HBMs) is recently (speaking w.r.t 2011) employed to model such an acquisition of abstract knowledge. Researchers showed that HBMs defined over graph- and grammar-based representations can discover the form of structure governing similarity in a domain. Structure of other forms such as tree, clusters, etc. can be represented as graphs, and the abstract principles (such as chain of info, ring/spectrum) underlying each form are expressed as simple grammatical rules for GROWING graphs of that form. (That means the abstraction does use some prior structure at the beginning, and continue to refine it through observations). It offers a top-down route to the origins of knowledge that contrasts sharply with the two classic approaches. Only HBMs thus seem suited to explaining the two most striking features of abstract knowledge in humans: that it can be learned from experience, and that it can be engaged remarkably early in life, serving to constrain more specific learning tasks.

## Open Questions

Although HBMs have addressed the acquisition of simple forms of abstract knowledge, they have only touched on the hardest subjects of cognitive development: framework theories for core common-sense domains such as intuitive physics, psychology, and biology.
Also, the project of reverse-engineering the mind must unfold over multiple levels of analysis (including computational (problem), algorithmic (solution procedure), implementation - the most important and practical level), but what has been discussed so far in this review is between computational and algorithmic levels. Key questions regarding the implementation level of human mind remained open.