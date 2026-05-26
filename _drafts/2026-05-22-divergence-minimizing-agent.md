---
title: Divergence Minimizing Agent
categories: Artificial Intelligence
date: 2026-05-22
math: true
published: true
---

There's nothing really original in this post, just generalizing distribution matching to AIXI and offering an interpretation.

***

AIXI maximizes expected utility over an infinite horizon:
$$\pi^*_\xi = \argmax_\pi \mathbb{E}_{\nu \sim w(\cdot),\, h \sim \nu^\pi} \left[ \sum_{t=1}^\infty \gamma^t r_t \right]$$
where $h = (a_1, o_1, r_1, a_2, o_2, r_2, \ldots)$ is an infinite trajectory of actions $a_t$, observations $o_t$, and rewards $r_t$.

We can consider another objective: minimizing the expected divergence between the distribution over trajectories $\nu^\pi$ and some target distribution $\chi$:
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

The KL divergence can be decomposed as a sum of one-step KL:
$$D_{KL}( \nu^\pi \| \chi ) = \sum_t \mathbb{E}_{h_{<t} \sim \mu^\pi(h_{<t})} [D_{KL}(\mu^\pi(a_t e_t | h_{<t}) \| P^\ast(a_t e_t | h_{<t}))],$$
can be infinite. To avoid this, we can introduce a discount factor $\gamma \in (0,1)$ and consider the discounted KL divergence instead:
$$\pi^* = \argmin_\pi \sum_t \gamma^t \, \mathbb{E}_{h_{<t} \sim \mu^\pi(h_{<t})} [D_{KL}(\mu^\pi(a_t e_t | h_{<t}) \| P^\ast(a_t e_t | h_{<t}))]
=
$$
where $h_t$ is the trajectory up to time step $t$.

We further assume that $U$ is a discounted sum of rewards at all time steps $t\in\mathbb N$: 
$$U(h) = \sum_{t=1}^\infty \gamma^t r_t$$
where $\gamma \in (0,1)$ is the discount factor and $r_t$ is the reward received at time step $t$. Note that $h = (a_1, o_1, r_1, a_2, o_2, r_2, \ldots)$ is a trajectory of actions $a_t$, observations $o_t$, and rewards $r_t$.


When $P^*(h)$ is a Boltzmann distribution over trajectories, i.e. $P^*(h) \propto e^{\beta U(h)} P_0(h)$ for some temperature $\beta > 0$ and base measure $P_0(h)$, this objective can be rewritten as:
$$\pi^* = \argmin_\pi D_{KL}(\mu^\pi(h) \| P_0(h)) - \beta \mathbb{E}_{h \sim \mu^\pi(h)}[U(h)].$$
Thus when $\beta \to \infty$, divergence minimization reduces to expected utility maximization.


looking for states where it's "fine" to be acting with high randomness


[1] Orseau et al., 2013. "Universal Knowledge-Seeking Agents for Stochastic Environments". https://www.hutter1.net/publ/ksaprob.pdf.