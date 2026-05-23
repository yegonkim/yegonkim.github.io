---
title: Divergence Minimizing Agent
categories: Artificial Intelligence
date: 2026-05-22
math: true
published: true
---

In this post I formalize and investigate agents that *minimize divergence* to a target distribution over trajectories, in general environments. This can be considered a generalization of utility maximizing agents, e.g. AIXI.

***

Reinforcement learning embodies the objective of expected utility maximization. That is, we are trying to find a policy $\pi$ that maximizes the expected utility $U$ over trajectories $h$:
$$\pi^* = \arg\max_\pi \mathbb{E}_{h \sim \mu^\pi(h)}[U(h)]$$
where $h = (a_1, o_1, r_1, a_2, o_2, r_2, \ldots)$ is an infinite trajectory of actions $a_t$, observations $o_t$, and rewards $r_t$, induced by the policy $\pi$ and environment $\mu$.

Here, we consider another objective: minimizing the divergence between the distribution over trajectories induced by the policy $\pi$ and a target distribution $P^*(h)$:
$$\pi^* = \arg\min_\pi D_{KL}(\mu^\pi(h) \| P^*(h)).$$
When $P^*(h)$ is a Boltzmann distribution over trajectories, i.e. $P^*(h) \propto e^{\beta U(h)} P_0(h)$ for some temperature $\beta > 0$ and base measure $P_0(h)$, this objective can be rewritten as:
$$\pi^* = \arg\min_\pi D_{KL}(\mu^\pi(h) \| P_0(h)) - \beta \mathbb{E}_{h \sim \mu^\pi(h)}[U(h)].$$
Thus when $\beta \to \infty$, divergence minimization reduces to expected utility maximization.

There is one caveat though: the KL divergence, which can be decomposed as a sum of one-step divergence at each time step $t$,
$$D_{KL}(\mu^\pi(h) \| P_0(h)) = \sum_t \mathbb{E}_{h_{<t} \sim \mu^\pi(h_{<t})} [D_{KL}(\mu^\pi(a_t e_t | h_{<t}) \| P_0(a_t e_t | h_{<t}))],$$
can be infinite. To avoid this, we can introduce a discount factor $\gamma \in (0,1)$ and consider the discounted KL divergence:
$$\pi^* = \arg\min_\pi \sum_{t=1}^\infty \gamma^t D_{KL}(\mu^\pi(a_t e_t | h_{<t}) \| P^*(a_t e_t | h_{<t}))$$
where $h_t$ is the trajectory up to time step $t$.

We further assume that $U$ is a discounted sum of rewards at all time steps $t\in\mathbb N$: 
$$U(h) = \sum_{t=1}^\infty \gamma^t r_t$$
where $\gamma \in (0,1)$ is the discount factor and $r_t$ is the reward received at time step $t$. Note that $h = (a_1, o_1, r_1, a_2, o_2, r_2, \ldots)$ is a trajectory of actions $a_t$, observations $o_t$, and rewards $r_t$.


