---
title: Divergence Minimization
categories: Artificial Intelligence
date: 2026-05-27
math: true
published: true
---

There's nothing really original in this post, just generalizing AIXI with the [distribution matching](https://www.beren.io/2022-11-27-Don't-argmax-distribution-match/) objective and offering an interpretation.

***

AIXI maximizes expected utility over an infinite horizon:

$$
\pi^*
= \arg \max_\pi \mathbb{E}_{\nu \sim w(\cdot),\, h \sim \nu^\pi} \left[ U(h) \right]
$$

where usually $U(h) = \sum_{t=1}^\infty \gamma^t r_t$.

We can consider another objective, of minimizing the expected divergence between $\nu^\pi$ and some target distribution $\chi$:

$$
\pi^*
= \arg \min_\pi \mathbb{E}_{\nu \sim w(\cdot)} \left[ D (\nu^\pi \parallel \chi) \right] 
.$$

The KL divergence $D_{KL}$ is a natural choice for $D$, since it can be decomposed into a sum of policy divergence and environment divergence terms at each time step:

$$
D_{KL} (\nu^\pi \parallel \chi)
=
\mathbb{E}_{h \sim \nu^\pi} \left[
\sum_{t=1}^\infty
    D_{KL}(\pi(\cdot \mid h_{<t}) \parallel \chi(\cdot \mid h_{<t}))
    +
    D_{KL}(\nu(\cdot \mid h_{<t} a_t) \parallel \chi(\cdot \mid h_{<t} a_t))
\right]
.
$$


To see the connection to utility maximization, consider $\chi(h) \propto e^{\beta U(h)} \chi_0(h)$, in which case distribution matching reduces to utility maximization as $\beta \to \infty$:

$$
\pi^\ast
=
\arg \min_\pi \mathbb{E}_{\nu \sim w(\cdot)} \left[ D_{KL}(\nu^\pi \parallel \chi_0) \right] 
- \beta\, \mathbb{E}_{\nu \sim w(\cdot),\, h \sim \nu^\pi} \left[ U(h) \right]
.$$

### Explicit Solution

One thing that's really nice about the objective described above is that the best strategy $\pi(\cdot \mid h_{<t})$ at time $t$ does not depend on the past profile $\pi(\cdot \mid h_{<1}), \dots, \pi(\cdot \mid h_{<t-1})$.
This generally doesn't hold for an arbitrary divergence $D$.
Another interesting case is the *pushforward* distribution matching objective $\pi^* = \arg \min_\pi \mathbb{E}\_{\nu \sim w(\cdot)} \left[ D\_{KL}(f_\\# \nu^\pi \parallel \chi) \right]$, where $f$ is some function that maps trajectories to some other space, e.g. $f(h) = o_T$. In that case, the best strategy at time step $t$ depends on its profile at time steps $<t$, making the problem [harder](https://arxiv.org/abs/2201.13259).

Anyways, thanks to this nice property, we can derive the soft Bellman equations:

$$
V^\ast (h_{<t}) = \log \sum_{a_t} \chi(a_t \mid h_{<t}) \exp \left(Q^\ast(h_{<t}, a_t) \right)
$$

$$
Q^\ast(h_{<t}, a_t) = \mathbb E_{\nu \sim w(\cdot \mid h_{<t} a_t)} \left[
    -D_{KL}(\nu(\cdot \mid h_{<t} a_t) \parallel \chi(\cdot \mid h_{<t} a_t))
    + 
    \mathbb E_{e_t \sim \nu(\cdot \mid h_{<t} a_t)} \left[
        V^\ast(h_{<t} a_t e_t)
    \right]
\right]
$$

with the optimal policy given by
$$
\pi^*(a_t \mid h_{<t}) \propto \chi(a_t \mid h_{<t}) \exp \left(Q^\ast(h_{<t}, a_t) \right).
$$

Note: we can [discount the environment divergence terms](https://www.hutter1.net/publ/ksaprob.pdf) $ D\_{KL}(\nu(\cdot \mid h\_{<t} a\_t) \parallel \chi(\cdot \mid h\_{<t} a\_t)) $ with $\gamma^t$ to avoid $\infty\text{’s}$.




### Interpretation


A common choice for $\chi_0$ is the uniform distribution over actions, or some base policy $\pi_0$ that we want to stay close to, while ignoring the environment divergence terms.
A more interesting choice is an expressive prior like the mixture distribution $\xi^\zeta$, where $\xi= \sum_{\nu\in\mathcal M} w(\nu) \, \nu$ and $\zeta= \sum_{\pi \in \mathcal P} \omega(\pi) \, \pi$. This way, the agent is looking for states where the agent itself can be accurately predicted by $\zeta$, and the environment accurately predicted by $\xi$ (similar to the [suprise-minimizing agent](https://michelevannucci.me/projects/master-thesis/) except that we are trying to distribution match to $\xi$ instead of maximizing evidence).
A simple approximation with neural nets would be to train a deep ensemble of probabilistic world models, and use negative [information gain](https://arxiv.org/pdf/1703.02910) as the reward.

When the optimal policy $\pi^\ast$ is not dominated by $\zeta$, it will seek out states where it can approximately model itself with elements in $\mathcal P$ (without losing too much reward). For instance, if $\mathcal P$ is the class of all computable policies, the agent would 1) be regularized towards computable policies, and also 2) seek out states $h_{<t}$ where one can perform decently, across most of the probable futures, by approximately following a computable function for time $\geq t$.
Similarly, max-entropy RL does not simply smoosh out the not-max-entropy optimal policy with uniform noise; it also encourages the policy to seek out states where it's "fine" to be a bit stochastic without incurring too much loss in the reward.
This also means that the agent will prefer states where it is robust to random perturbations.
Finally, it's interesting to note that a highly expressive base policy $\pi_0$ can be viewed as a mixture $\zeta$ of simpler policies, e.g. a large language model with [multiple personas](https://www.anthropic.com/research/persona-selection-model).
