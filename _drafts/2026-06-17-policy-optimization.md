---
title: Policy Optimization
categories: ["Artificial Intelligence"]
date: 2026-06-17
math: true
published: true
---

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

**Environment-policy process.**
Suppose we have an environment generator $\Mu: (\mathcal P \times \mathcal M)^\ast \to \mathcal \Delta \mathcal M$ and a policy generator $\Pi: (\mathcal P \times \mathcal M)^\ast \to \mathcal \Delta \mathcal P$, where $\mathcal P$ is a countable class of policies and $\mathcal M$ a countable class of environments.
Together, they induce the environment-policy process $\Mu^\Pi$, defined by

$$
\Mu^\Pi(\pi\!\mu_{1:n}) = \prod_{i=1}^n \Pi(\pi_i \mid \pi\!\mu_{<i}) \Mu(\mu_i \mid \pi\!\mu_{<i})
.
$$

where $\pi\!\mu_i$ is shorthand for $(\pi_i, \mu_i)$.
Note that $\Pi$ and $\Mu$ generate $\pi_i$ and $\mu_i$ simultaneously, instead of one after another.
This is akin to a multi-agent system with two agents $\Mu$ and $\Pi$.

<!-- If we ever need a class of environment generators and policy generators, we will denote them by $\mathfrak M$ and $\mathfrak P$, respectively. -->

Note:
- I started with a much simpler formulation, where $\mu_i$ only depends on $\mu_{<i}$. But then I realized that this is not very realistic, because engineers can be informed by the trained policies, to choose which task the policies should be trained on next. The current formulation doesn't seem very realistic either because $\mu_i$ should be able to depend on $\pi_i$ as well. The reason why I chose the current formulation is because the notion of asymptotic optimality becomes ambiguous and messy with the more realistic formulation.
- It's possible that the generality of the current formulation doesn't get us anywhere. In that case, we should go back to the simpler formualation where $\mu_i$ only depends on $\mu_{<i}$.
- I've been side-tracked by the idea of deceptive alignment (alignment faking), and that's why I started thinking about the more general formulation. But the more I think about it, it's not so obvious whether it is feasible to say anything interesting about deceptive alignment with our overall approach. After all, Hubinger et al. believe that the deceptively aligned policies can emerge due to inductive bias, but likely not due to an external incentive.

### Standard Policy Optimization

Given a sequence of environments $\mu_{<n}$,
a standard "policy optimization" procedure has the objective function

$$
J(\pi)
= \frac{1}{n-1} \sum_{i=1}^{n-1} V^{\pi}_{\mu_i}
.
$$

We can consider the Gibbs posterior

$$
\omega_n(\pi) \propto \omega(\pi) \exp\left( \beta_n J(\pi) \right)
,
$$

where $\omega(\pi)$ is a Gibbs *prior* that encodes some inductive bias.

Then, standard policy optimization $\Pi$ is defined as:

$$
\Pi(\pi \mid \pi\!\mu_{<n}) = \omega_n(\pi)
.
$$

Notes:
- This is a model of perfect optimization, unlike in practice where we have sloppy optimization due to limited compute and data.
- The objective function can be a sum of RL objectives and supervised objectives, e.g., cross-entropy loss. We focus on the RL objective here, but I expect most of the analysis to generalize to the supervised objective.

## Asymptotic Optimality

What does it mean for a policy generator $\Pi$ to be *effective*?
This is related to the question of which policy generators we can expect people to use in practice, and somewhat related to how interpretable/predictable the behavior of $\Pi$ is.
<!-- 
We present the notion of *asymptotic optimality* for policy generators.
Given $\pi_n$, define $\xi_n = \mathbb E_{\mu_n \sim \Mu(\cdot \mid \pi\!\mu_{<n} \pi_n)} \left[ \mu_n \right]$ to be the mixture of environments from $\Mu(\cdot \mid \pi\!\mu_{<n} \pi_n)$. 
We define the asymptotic optimality of $\Pi$ in $\Mu$ as follows:

$$
\lim_{n \to \infty}
\mathbb E_{\pi_n \sim \Pi(\cdot \mid \pi\!\mu_{<n})}
\left[
% \mathbb E_{\mu_n \sim \Mu(\cdot \mid \pi\!\mu_{<n}\pi_n)}
% \left[
V^\ast_{\xi_n}
-
V^{\pi_n}_{\xi_n}
% \right]
\right]
=0
,
$$

with $\Mu^\Pi$-probability 1. -->

Let $\xi_n = \mathbb E_{\mu_n \sim \Mu(\cdot \mid \pi\!\mu_{<n})} \left[ \mu_n \right]$ be the mixture of environments output by $\Mu$ at time $n$.
Similarly, let $\zeta_n = \mathbb E_{\pi_n \sim \Pi(\cdot \mid \pi\!\mu_{<n})} \left[ \pi_n \right]$.
We define the asymptotic optimality of $\Pi$ when coupled with $\Mu$ as follows:

$$
\lim_{n \to \infty} V^\ast_{\xi_n} - V^{\zeta_n}_{\xi_n} = 0
,
$$

with $\Mu^\Pi$-probability 1.

This is equivalent to saying that $\Pi$ converges to the best response against $\Mu$, with respect to the *myopic* objective $V^{\pi_n}_{\mu_n}$.
We can also imagine a non-myopic objective, in which case an effective $\Pi$ would generate $\pi_n$ that steers $\Mu$ toward generating more favorable environments in the future, e.g., environments that are more generous with their rewards. But not only could this be disastrous from the safety perspective, it's also not very useful (or at least I don't see how it could be useful).

### IID Environments

Suppose $\Mu$ generates i.i.d. environments, that is,

$$
\Mu(\mu_i \mid \pi\!\mu_{<i} \pi_i) = \Mu(\mu_i)
$$

for all $i$.
Also, let $\Pi$ be the standard policy optimization, as described above.

As $n \to \infty$, $\hat \xi_{n} = \frac{1}{n-1}(\mu_1 + \dots + \mu_{n-1})$ converges to $\xi = \mathbb E_{\nu \sim \Mu} \left[ \nu \right]$.
Since
$
J(\pi)
= V^{\pi}_{\hat \xi_{n}}
$,
the Gibbs posterior $\omega_n$ concentrates on optimal policies in $\xi$. Thus,
$
\lim_{n \to \infty} V^\ast_\xi - V^{\zeta_n}_{\xi} = 0
$
holds
$\Mu^\Pi$-almost surely, and $\Pi$ is asymptotically optimal.
A formal proof with proper conditions on $\beta_n$ can be found in **Appendix A.1**.

### General Environment Process

In general, standard policy optimization is not asymptotically optimal when $\Mu$ is not i.i.d.

Consider, for instance, a simple environment generator $\Mu$ that outputs $\mu_1$ at odd times and $\mu_2$ at even times.

$$
\mu_1 \to \mu_2 \to \mu_1 \to \dots
$$

Standard policy optimization will converge to a policy that is optimal in the mixture environment $\xi = \frac{1}{2} \mu_1 + \frac{1}{2} \mu_2$, which is not necessarily optimal in either $\mu_1$ or $\mu_2$.

### Distinguishability

We said an optimal policy in $\xi = \frac{1}{2} \mu_1 + \frac{1}{2} \mu_2$ is not necessarily optimal in $\mu_1$ or $\mu_2$. But can it be optimal? What if $\mu_1$ and $\mu_2$ are completely distinguishable from the start, e.g., because they output some rich contexts $c_1$ and $c_2$ at the beginning?
Then,

$$
V^{\pi}_{\xi}
=
\frac{1}{2} V^{\pi}_{\mu_1} + \frac{1}{2} V^{\pi}_{\mu_2}
=
\frac{1}{2} V^{\pi(\cdot \mid c_1)}_{\mu_1} (c_1) + \frac{1}{2} V^{\pi(\cdot \mid c_2)}_{\mu_2} (c_2),
$$

and since $\pi(\cdot \mid c_1)$ and $\pi(\cdot \mid c_2)$ are independent,

$$
V^\ast_\xi = \frac{1}{2} V^\ast_{\mu_1} + \frac{1}{2} V^\ast_{\mu_2}
.
$$

Similarly, suppose $\Mu$ generates environments $\nu$ that output a unique context $c_\nu$ at the beginning. Let $\hat f_n(\nu)$ denote the empirical frequency of $\nu$ until time $n-1$. If eventually
$\Mu(\nu \mid \pi\!\nu_{<n}) \leq C \hat f_n(\nu)$
for all $\nu$, then

$$
V^{\ast}_{\xi_n} - V^{\zeta_n}_{\xi_n}
= \sum_{\nu} \Mu(\nu \mid \pi\!\nu_{<n}) \bigl(V^{\ast}_{\nu} - V^{\zeta_n}_{\nu}\bigr)
\leq C \sum_{\nu} \hat f_n(\nu) \bigl(V^{\ast}_{\nu} - V^{\zeta_n}_{\nu}\bigr)
\to 0
$$

so $\Pi$ is asymptotically optimal.


### Model-based policy optimization

We showed above that standard policy optimization isn't asymptotically optimal in the general case.
Is there a policy generator that is asymptotically optimal in all environment generators $\Mu$ from some class $\mathfrak M$?
The obvious solution would be to guess the next environment $\mu_n$ based on the history $\pi\!\mu_{<n}$, and then optimize the policy $\pi_n$ for the predicted environment.

If we have some prior $\mathfrak w$ over the class $\mathfrak M$ of environment generators, we can perform Bayesian inference to obtain the posterior $\mathfrak w_n$ given the history $\pi\!\mu_{<n}$.
Then, we perform standard policy optimization on the mixture environment $\hat \xi_n = \mathbb E_{\Mu \sim \mathfrak w_n, \, \mu \sim \Mu(\cdot \mid \pi\!\mu_{<n})} \left[ \mu \right]$.
Since $\hat \xi_n - \xi_n \to 0$ by merging of opinions, we can probably show that the resulting policy generator $\Pi$ is asymptotically optimal, under some conditions on $\beta_n$.

### Learning to do model-based policy optimization

It's possible that under some extra conditions, providing $\pi\!\mu_{<n}$ as a context to $\pi_n$ and running standard policy optimization is asymptotically optimal. This is because $\xi_n$ can eventually be inferred from $\pi\!\mu_{<n}$ with arbitrary precision, and thus $\pi_n$ can learn to infer $\xi_n$ from $\pi\!\mu_{<n}$ and optimize for it — essentially learning to do the model-based policy optimization within itself.


## Mesa-optimizers

Note:
- This section is pretty unrelated to the previous section on asymptotic optimality. In fact, we only consider a single training environment $\mu$.

How does a policy, obtained through policy optimization on some environment $\mu$, behave in a state never encountered during training on $\mu$? For instance, $\mu$ is a maze environment where the goal is to reach a red box. How will the policy trained on $\mu$ behave in $\mu'$, where the maze has red circles instead?
Would it still act in a goal-directed manner? If so, can the goal be predicted from the goal in $\mu$?

Suppose $\omega$ is the simplicity prior
$
\omega(\pi) = \omega(\pi) \propto 2^{-K(\pi)}
$,
which assigns higher probability to simpler policies in the class $\mathcal P$.
Let's also define the simplicity prior $w(\nu) \propto 2^{-K(\nu)}$ on environments in class $\mathcal M$.

Let $\pi^\dagger_{-}: \mathcal M \to \mathcal P$ be a mapping from environments to policies, such that $\pi^\dagger_\nu$ is near-optimal in $\nu$ for all $\nu \in \mathcal M$,

$$
V^{\pi^\dagger_\nu}_\nu \ge V^\ast_\nu - \varepsilon
,
$$

and $\pi^\dagger_\nu$ is as about simple as $\nu$, in the sense that

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
perhaps we should restrict to histories that are not too "far away" from the ones seen in training?
-->

<!-- Let's make a strong assumption:
there exists a wrapper $\pi^\dagger_{-}: \mathcal M \to \mathcal P$ that maps an environments to a planner that is optimal in that environment, and

$$
-\log w(\nu) \overset{+}{=} -\log \omega(\pi^\dagger_\nu) 
.
$$

Unfortunately, this is blatantly false in general (since an optimal policy can be very simple for some complex environments), but for now let's just go along with it and see where it leads us.

Let's also assume that every policy $\pi$ is equal to $\pi^\dagger_\nu$ for some environment $\nu$.

Let $\mathcal M_\mu$ be the set of environments that are indistinguishable from $\mu$ in the sense that they have the same probability distribution over histories as $\mu$:

$$
\mathcal M_\mu = \{ \nu \in \mathcal M : \forall h_{<t} \ \mu(h_{<t}) = \nu(h_{<t}) \}
$$

This doesn't mean that $\mu = \nu$, because they can have different transition functions and utility functions on histories that have *zero probability* under $\mu$.
Still, we know that $\pi^\dagger_\nu$ is optimal in $\mu$.

Let $\beta$ be very, very large, so that the Gibbs posterior $\omega_\beta$ concentrates on the optimal policies in $\mu$.
Then, we can show that

$$

$$

Let $\tilde \mu = \argmin_{\nu \in \mathcal M_\mu} K(\nu)$ be the simplest environment in $\mathcal M_\mu$.
Then, by the assumption above, we have  -->





<!-- 
Suppose also that all the environments $\mu$ start with a percept, or context, $e_0$, before the first action $a_1$. And also suppose that there exists a set of percepts $\mathcal E^\empty$ such that 

$$
\mu(e_0 \in \mathcal E^\empty) < \delta_1
.
$$

That is, the environment $\mu$ almost never outputs a percept from $\mathcal E^\empty$ at the beginning.

Then, consider the set of environments $\nu$ that are concentrated on $\mathcal E^\empty$ at the beginning, i.e., $\nu(e_0 \in \mathcal E^\empty) > 1 - \delta_2$. The mixture

$$
\tilde \mu = w_1 \mu + w_2 \nu
$$

can be simpler than $\mu$, especially if $\mathcal E^\empty$ is a small set. (Not sure about this assumption either, because $\mathcal E^\empty$ is usually not that small.)

We can bound the value of an arbitrary policy in $\tilde \mu$ as follows:

$$
V^\pi_{\tilde \mu} = V^\pi_{\tilde \mu}(e_0 \in \mathcal E^\empty) \tilde \mu(e_0 \in \mathcal E^\empty) + V^\pi_{\tilde \mu}(e_0 \notin \mathcal E^\empty) \tilde \mu(e_0 \notin \mathcal E^\empty)
\le V^\pi_{\tilde \mu}(e_0 \in \mathcal E^\empty) + \delta_1
$$ -->




<!-- 
Suppose we have an environment $\mu$, and consider the Gibbs posterior

$$
\omega'(\pi) \propto \omega(\pi) \exp\left( \beta V^\pi_\mu \right)
.
$$



It's reasonable to assume that there exists some mapping $\pi^\dagger_{-}: \mathcal M \to \mathcal P$ such that a relatively simple environment $\mu$ is mapped to a relatively simple policy $\pi^\dagger_\mu$, and such that $\pi^\dagger_\mu$ is near-optimal in $\mu$.
More formally, there exist small constants $c$ and $\varepsilon$ such that for every $\mu$,

$$
-\log \omega(\pi^\dagger_\mu) \le -\log w(\mu) + c
,
$$

and

$$
V^{\pi^\dagger_\mu}_{\mu} \ge V^{\ast}_{\mu} - \varepsilon
.
$$

For example, one can consider a planner that wraps around an arbitrary environment $\mu$. The planner has a fixed number of bits, meaning that it incurs only a constant overhead in complexity $K$.

We can expect that there are *many* such mappings $\pi^{\dagger}_-$. For instance, planners can vary in rules for tie-breaking, or tolerance ($\approx$ planning horizon). We denote the set of all such mappings by $\Pi^{\dagger}_{\varepsilon, c}$.

suppose xi and nu being close together implies pi^dagger_xi is good in nu.

what do we mean by close together? nu has high weight in xi? what is weight? minimal w such that 1/w * xi >= nu ?
also, xi should be simple so that pi^dagger_xi is simple

but it's possible that there is an optimal policy pi^* in nu that's simple as hell. how do we prevent that from happening?
or at least, how do we make that "rare" so that pi^dagger_xi for lots of xi can dominate! of course, we would also have to make sure pi^dagger_xi does not overlap with pi^*.
this is possibly the irl thing. "if there's an optimal policy pi^*_nu that's simple, then we can equate it with 

V_nu^pi >= 1/w * (V^pi_xi - (1-w))

At the core, we want xi such that: pi that is near-optimal in xi is guaranteed to be near-optimal in nu. and also, xi should be simple.

when can policy optimal in xi be optimal in nu ?  -->




<!-- 
### Is AIT necessary to model this?

A possible argument for why trained systems will favor optimizers over memorizers is the following: 


### Hybrid systems


It's possible that future AI systems become more hybrid and messier (more evolutionary-outcome-like) than current LLMs.
For instance, the system could have an explicitly programmed planning module, or it could be a multi-agent system.
In such cases, we might not be performing end-to-end optimization on the whole system.
How much would our use of Gibbs posterior with simplicity prior be justified?

I would imagine model-free policy optimization to still play an important role in such messy AI systems,  -->


## Appendix

#### A.1. Proof of asymptotic optimality in the i.i.d. case


Choose $\beta_n$ such that

$$
\log n\ll \beta_n\ll n.
$$

For example, $\beta_n=\sqrt n$ works.
Assume that there exists some policy $\pi^\ast \in \mathcal P$ that is optimal in $\xi$. (We can relax this assumption to a sequence of policies $\pi^\ast_n$ that are $\varepsilon_n$-optimal, with $\varepsilon_n \searrow 0$.)

For a distribution $q$ over $\mathcal P$, write
$
V^q_\nu = \sum_{\pi\in\mathcal P} q(\pi) V^\pi_\nu
$.
The Gibbs posterior $\omega_n$
has the variational characterization

$$
\omega_n = \arg \max_q  V^q_{\hat \xi_n}-\frac{1}{\beta_n}\mathrm{KL}(q\Vert\omega).
$$

By two-sided Hoeffding-style PAC-Bayes bound (that follows from Theorem 2.1 in ["User-friendly Introduction to PAC-Bayes Bounds"](https://arxiv.org/pdf/2110.11216)),
with probability at least $1-\delta$, for all posteriors $q$,

$$
|V^q_\xi-V^q_{\hat \xi_n}|
\le
\frac{\mathrm{KL}(q\Vert\omega)+\log(2/\delta)}{\beta_n}
+
\frac{\beta_n}{8(n-1)}.
$$

Combined with the variational characterization of $\omega_n$, for any distribution $q$,

$$
V^q_\xi - V^{\omega_n}_\xi
\le
2\frac{\mathrm{KL}(q\Vert\omega)+\log(2/\delta)}{\beta_n}
+
\frac{\beta_n}{4(n-1)}.
$$

Taking $q=\delta_{\pi^\ast}$ gives

$$
V^\ast_\xi-V^{\omega_n}_\xi
\le
2\frac{\log\frac{1}{\omega(\pi^\ast)}+\log(2/\delta)}{\beta_n}
+
\frac{\beta_n}{4(n-1)}.
$$

Now set $\delta_n=n^{-2}$. By Borel-Cantelli, the bound holds eventually almost surely. Since $\log(2/\delta_n)=O(\log n)$, $\log n\ll \beta_n$, and $\beta_n\ll n$, the right-hand side goes to $0$. Therefore

$$
\lim_{n\to\infty} V^\ast_\xi-V^{\omega_n}_\xi\to0
$$

almost surely.

