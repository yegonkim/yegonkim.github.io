---
title: Policy Optimization
categories: ["Artificial Intelligence"]
date: 2026-06-17
math: true
published: true
---

### Motivation

How does a policy, obtained through policy optimization on some environment $\mu$, behave in a state never encountered during training on $\mu$? For instance, $\mu$ is a maze environment where the goal is to reach a red box. How will the policy trained on $\mu$ behave in $\mu'$, where the maze has red circles instead of red boxes?
Would it still act in a goal-directed manner? If so, can the goal be extrapolated (in a predictable way) from the goal in $\mu$?


### 

We define a policy to be a stochastic mapping $\pi: (\mathcal A \times \mathcal E)^\ast \to \Delta \mathcal A$ from histories to actions, as usual.
We define an environment $\mu$ to be a pair $(T_\mu, u_\mu)$. The first component is the transition function $T_\mu: (\mathcal A \times \mathcal E)^\ast \times \mathcal A \to \Delta \mathcal E$, and the second component is the utility function $u_\mu: (\mathcal A \times \mathcal E)^\infty \to [0,1]$ that scores full trajectories.
This allows us to model situations where the reward is not observed during interaction.

The value of a policy $\pi$, in an environment $\mu$, is defined as the expected utility:

$$
V^{\pi}_{\mu} = \mathbb E_{h \sim \mu^\pi} \left[ u_\mu(h) \right]
.
$$

A mixture environment $\xi = \sum_\nu w(\nu) \nu$ is defined with the transition function $T_\xi(e_t \mid h_{<t} a_t) = \sum_\nu w(\nu \mid h_{<t}) T_\nu(e_t \mid h_{<t} a_t)$ and the utility function $u_\xi(h) = \sum_\nu w(\nu \mid h) u_\nu(h)$.
We will abuse the notation and write $\mu$ for $T_\mu$ when there is no ambiguity.

### Policy Optimization

Given a bunch of environments $\mu_1, \dots, \mu_n$,
a standard "policy optimization" procedure has the objective function

$$
J(\pi)
= \frac{1}{n} \sum_{i=1}^{n} V^{\pi}_{\mu_i}
.
$$

This is equivalent to optimizing the value in the Bayesian mixture environment $\xi = \frac{1}{n} \sum_{i=1}^{n} \mu_i$.

$$
J(\pi)
= V^{\pi}_{\xi}
.
$$

The objective can have multiple optima. Which one will a learner choose? This depends on the *inductive bias* of the learner.
The Gibbs posterior provides a principled model of a learner with an inductive bias expressed by some prior $\omega(\pi)$, and with the objective $J(\pi)$:

$$
\omega_\beta (\pi) \propto \omega(\pi) \exp \left( \beta J(\pi) \right)
,
$$

where $\omega(\pi)$ is a Gibbs prior that encodes some inductive bias.
As $\beta \to \infty$, $\omega_\beta$ concentrates on the set of optimal policies $\pi \in \Pi^\ast_\xi$, with weights proportional to $\omega(\pi)$.

Notes:
- This is a model of perfect optimization, unlike in practice where we have sloppy optimization due to limited compute and data.
- The objective function can be a sum of diverse objectives, including supervised learning objectives, e.g., cross-entropy loss. We focus on the RL objective here.

## Learned Planning

How does a policy, obtained through policy optimization on some environment $\mu$, behave in a state never encountered during training on $\mu$? For instance, $\mu$ is a maze environment where the goal is to reach a red box. How will the policy trained on $\mu$ behave in $\mu'$, where the maze has red circles instead of red boxes?
Would it still act in a goal-directed manner? If so, can the goal be extrapolated (in a predictable way) from the goal in $\mu$?

This depends on the inductive bias, i.e. the Gibbs prior $\omega$, and the environment $\mu$.
The original mesa-optimizers paper by Hubinger et al. discuss other factors such as sloppy optimization and limited exploration, etc., but we ignore those factors here.




Suppose $\omega$ is the simplicity prior
$
\omega(\pi) = \omega(\pi) \propto 2^{-K(\pi)}
$,
which assigns higher probability to simpler policies in the class $\mathcal P$.
Let's also define the simplicity prior $w(\nu) \propto 2^{-K(\nu)}$ on environments in class $\mathcal M$.

Let $\pi^\dagger_{-}: \mathcal M \to \mathcal P$ be a planner that maps environments to policies, such that $\pi^\dagger_\nu$ is near-optimal in $\nu$ for all $\nu \in \mathcal M$,

$$
V^{\pi^\dagger_\nu}_\nu \ge V^\ast_\nu - \varepsilon
,
$$

and $\pi^\dagger_\nu$ is about as simple as $\nu$, in the sense that

$$
-\log \omega(\pi^\dagger_\nu) \overset{+}{=} -\log w(\nu)
.
$$

Suppose also that every near-optimal policy in $\mu$ can be represented as $\pi^\dagger_\nu$ for some environment $\nu$. (This is a very strong assumption.)

Let $\mathcal M_\mu$ be the set of all environments $\nu$ such that a policy being near-optimal in $\nu$ implies the same policy being near-optimal in $\mu$.
In other words,

$$
\mathcal M_\mu
=
\{
\nu \in \mathcal M
:
\forall \pi \in \mathcal P, \ 
V^\pi_\nu \ge V^\ast_\nu - \varepsilon
\implies
V^\pi_\mu \ge V^\ast_\mu - \varepsilon
\}
.
$$

I want to show that for large $\beta$, the Gibbs posterior $\omega_\beta(\pi) \propto \omega(\pi) \exp\left( \beta V^\pi_\mu \right)$ is approximately the pushforward of the distribution $w_\mu (\nu) \propto w(\nu) \mathbb I [\nu \in \mathcal M_\mu]$ under the mapping $\pi^\dagger_{-}$.

<!-- 
https://www.mdpi.com/1099-4300/28/6/596
how should we interpret meta-RL in our framework?
i guess the inner RL algorithm should be considered 
 -->

<!-- 
how do we interpret a policy that does supervised learning with the given data (context) ?
i guess it's just solomonoff induction over environment.
 -->

<!-- 
https://www.lesswrong.com/posts/WmBukJkEFM72Xr397/mesa-search-vs-mesa-control

i want my framework to be able to capture both mesa search and mesa control.
this would be by not fixating ourselves on the AIXI style planner, but any general wrapper that satisfies some notion of optimality, e.g.

 -->

<!--
perhaps we should restrict to histories that are not too "far away" from the ones seen in training?
-->


<!-- 
deceptive alignment is a special case of utility function misgeneralization.
It happens when
K(deceptive utility) < K(true utility)
 -->


### One-step environment

Let's start proving some results in the simplest case, and slowly build up to more general cases.

Consider a simple one-step choice task $\mu$. At the start of the interaction, it shows the agent two objects, such as a cat and a dog. The agent must then choose either the object on the left or the object on the right. After the choice is made, the agent receives the reward assigned to the object it selected, according to the reward function $r$.

More formally, let $\mathcal A = \{ \text{left}, \text{right} \}$ be the action space and $\mathcal E = \mathcal O \times \mathcal O$ be the percept space where $\mathcal O$ is a set of objects, e.g., $\mathcal O = \{ \text{cat}, \text{dog}, \text{bird}, \text{fish}, \text{zebra} \}$. 
Let $r: \mathcal O \to [0,1]$ be a reward function that assigns a reward to each object.
The environment $\mu$ first outputs a pair of objects $(o_{\text{left}}, o_{\text{right}}) \in \mathcal O \times \mathcal O$ as its percept. The agent then chooses an action $a \in \mathcal A$, and receives the reward $r(o_a)$, where $o_a$ is the object corresponding to the chosen action.
This reward is the utility of the interaction, so $u_\mu(o_{\text{left}}, o_{\text{right}}, a)=r(o_a)$.

Let's restrict $\mathcal M$ to all one-step choice tasks with the same action space $\mathcal A$ and percept space $\mathcal E$, but with different reward functions $r_\nu: \mathcal O \to [0,1]$.



The aim is to show that the Gibbs posterior $\omega_\beta$ concentrates on policies that are near-optimal in some environment $\nu \in \mathcal M_\mu$, as $\beta \to \infty$.

