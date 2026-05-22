---
title: Divergence Minimizing Agent
categories: Artificial Intelligence
date: 2026-05-22
math: true
published: true
---

In this post I'll formalize and investigate agents that optimize the objective of *divergence minimization* in general environments, which is, in the limit of temperature $\lambda \to 0$, *utility maximizing* like AIXI.

***

Reinforcement learning usually embodies the objective of expected utility maximization. That is, we are trying to find a policy $\pi$

[](https://arxiv.org/abs/2009.01791)

