---
title: Divergence Minimizing Agent
categories: Artificial Intelligence
date: 2026-05-22
math: true
published: true
---

There's nothing really original in this post, just generalizing distribution matching to AIXI and offering an interpretation.

***

AIXI maximizes expected discounted return over an infinite horizon:
$$\pi^* = \argmax_\pi \mathbb{E}_{\nu \sim w(\cdot),\, h \sim \nu^\pi} \left[ \sum_{t=1}^\infty \gamma^t r_t \right].$$

We can consider another objective: minimizing the expected divergence between the distribution over trajectories $\nu^\pi$ and some target distribution $\chi$ (it also has a useful one-step decomposition as shown on the right hand side):
$$\pi^*
= \argmin_\pi \mathbb{E}_{\nu \sim w(\cdot)} [ D_{KL}(\nu^\pi \| \chi) ] 
= \argmin_\pi \mathbb{E}_{\nu \sim w(\cdot)} \left[
    \sum_{t=1}^\infty
    \mathbb{E}_{h_{<t} \sim \nu^\pi(h_{<t})} \left[
    D_{KL}(\nu^\pi(a_t e_t | h_{<t}) \| \chi(a_t e_t | h_{<t}))
    \right]
\right] 
.$$
Actually, this can be $\infty$ so let's discount it as well:
$$\pi^*
= \argmin_\pi \mathbb{E}_{\nu \sim w(\cdot)} \left[
    \sum_{t=1}^\infty
    \gamma^t
    \mathbb{E}_{h_{<t} \sim \nu^\pi(h_{<t})} \left[
    D_{KL}(\nu^\pi(a_t e_t | h_{<t}) \| \chi(a_t e_t | h_{<t}))
    \right]
\right] 
.$$

When $\chi$ is a Boltzmann distribution over trajectories, i.e. $\chi(h) \propto e^{\beta U(h)} \chi_0(h)$ with $U(h)=\sum_{t=1}^\infty r_t$, this objective can be rewritten as:
$$\pi^* = \argmin_\pi D_{KL}(\mu^\pi(h) \| P_0(h)) - \beta \mathbb{E}_{h \sim \mu^\pi(h)}[U(h)].$$
Thus when $\beta \to \infty$, divergence minimization reduces to expected utility maximization.


looking for states where it's "fine" to be acting with high randomness


[1] Orseau et al., 2013. "Universal Knowledge-Seeking Agents for Stochastic Environments". https://www.hutter1.net/publ/ksaprob.pdf.