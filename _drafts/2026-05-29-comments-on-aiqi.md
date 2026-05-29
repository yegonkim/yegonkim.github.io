---
title: Comments on Model-Free UAI
categories: Artificial Intelligence
date: 2026-05-29
math: true
published: true
---

Some further thoughts on my recent work ["A Model-Free Universal AI"](https://arxiv.org/abs/2602.23242).

***

To summarize, [my recent work](https://arxiv.org/abs/2602.23242) shows that on-policy [distributional](https://arxiv.org/abs/1707.06887) [Monte Carlo control](http://incompleteideas.net/book/ebook/node53.html) with epsilon-greedy exploration is asymptotically $\varepsilon$-optimal in (this is the cool part) *general environments*, not just MDPs or POMDPs.


Another key contribution, which I only discovered after rounds of discussion with Cole Wyeth, is the proof of asymptotic optimality for [Self-AIXI](https://proceedings.neurips.cc/paper_files/paper/2023/hash/56a225639da77e8f7c0409f6d5ba996b-Abstract-Conference.html) without ad-hoc assumptions made in its original paper.


epsilon exploration -> i was just trying to prove the damn theorem
turns out it's been proposed as a simple solution to action counterfactuals



speed of convergence
altohugh not meant to be practical in the first place, one can imagine how slow AIQI, due to its dumb epsilon-greedy exploration. one can apply ksa to speed up learning.
straightforward for self-aixi (KL of xi^zeta and nu^pi)
not so straightforward for AIQI.


how it changed my model of capable agents
it's probably impossible to do policy-search-like thing in general env, the target is nonstationary so you can't generally guarantee that a profile that did well at an earlier time will do well later on.
value learning is necessary.
but we will likely do policy search inside a world model, with the learned value of the final state as the target. policy search inside a world model is, in the limit, essentially the same as planning inside a world model, but it can be more efficient in practice, and it also allows us to do things like regularization towards some base policy.
more concretely, I tend to view each action $a_i$ and percept $e_i$ in the general environment formulation as a $T$-step policy $\pi\_i$ and environmental feedback $\nu^{\pi_i} (e\_{iT:i(T+1)} \mid h\_{<t})$.


