---
title: When Analysis by Synthesis Fails (and When It Succeeds)
categories: Artificial Intelligence
date: 2025-12-19
math: true
published: true
---

if you try to capture any knowledge about the world in a prior, it's probably an 'evident' one that can be inferred from the data by a solomonoff inductor. so unless your machine is severely constrained in learning some kind of a general prior, it's probably not very useful to hard-code priors into it.

if you believe that certain 'obvious to human facts' about the world are not learnable from data, you should probably reconsider your beliefs.
usually, there's a certain 'transfer' between tasks that allows learning those facts from data alone. a.k.a. meta-learning
inferring causal graph is also possible from data alone, given enough other knowledge about the world. even without interventions. (of course there is a limit, but that limit is much better than what humans can do)

so what should we do?
we should try to find inductive methods, and NOT priors, that are missing in current general learning systems. those that bring them one step closer to solomonoff induction.
e.g. pattern matching, program synthesis, long term memory retrieval, etc.


what are some inductive methods existing in neural nets?
1. deep computation (albeit fixed)
2. in context learning with 