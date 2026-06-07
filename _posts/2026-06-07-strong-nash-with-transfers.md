---
title: Strong Nash with Transfers
categories: Artificial Intelligence
date: 2026-06-07
math: true
published: true
---

I don't know much about game theory, so 
this is an attempt to get familiar with it, through the question "in what sense can players who bear the cost of their local objectives coalesce to optimize a coherent global objective?"
There's nothing really insightful here, and the result is much less general than I had hoped for.
I am half-forcing myself to post it nevertheless for the sake of posting something regularly.

***

Consider a deterministic feedforward model:

$$
y = f_L \circ f_{L-1} \circ \cdots \circ f_1 (x)
$$

where $f_l$ must be chosen from the hypothesis class $\mathcal{H}_l$.
<!-- Usually, each $\mathcal{H}_l$ is a rather restricted class, but their composition can express a much wider variety of functions. -->
<!-- For instance, it could be the class of all linear functions from $\mathbb{R}^n$ to $\mathbb{R}^m$ (perhaps composed with a nonlinear activation function), the class of Boolean circuits of depth $\leq \tilde d$ from $\mathbb{B}^n$ to $\mathbb{B}^m$, the class of Turing machines with running time $\leq \tilde t$, etc.
By composing these functions, we can express a wider range of functions: multi-layer perceptrons, circuits of depth $\le L \tilde d$, programs with running time $\le L \tilde t$. -->

Given a dataset of input-output pairs $\\{ ( x^{(i)}, y^{(i)} )  \\}_{i=1}^N$, our objective is to minimize:

$$
\mathcal L [f_1, \dots, f_L] = \sum_{i=1}^N \ell(y^{(i)}, f_L \circ f_{L-1} \circ \cdots \circ f_1 (x^{(i)})) + \sum_{l=1}^L c (f_l)
$$

where

$$
\ell(y, \hat y) = \begin{cases}
0 & \text{if } y = \hat y \\
\infty & \text{if } y \neq \hat y
\end{cases}
$$

is the zero-infinity loss and $c(f_l)$ is some penalty depending on the choice of $f_l$.

We can actually rewrite the objective into a sum of more "local" objectives by introducing intermediate variables $z_l$:

$$
\mathcal F [f_1, \dots, f_L, z_1, \dots, z_{L-1}]
=
\sum_{i=1}^N
\left[
    \ell(y^{(i)}, f_L (z_{L-1}^{(i)})) + \ell(z_{L-1}^{(i)}, f_{L-1} (z_{L-2}^{(i)})) + \cdots + \ell(z_1^{(i)}, f_1 (x^{(i)}))
\right]
+
\sum_{l=1}^L c (f_l).
$$

### Nash Equilibrium

Let's think of each $z_l$ and $f_l$ as a player in a game where ${z_l}$ bears the cost of

$$
\ell(z_{l+1}, f_{l+1} ({\color{red}{z_l}})) + \ell({\color{red}{z_l}}, f_l (z_{l-1})) = \ell_{l+1} + \ell_l,
$$

and ${f_l}$ bears the cost of

$$
\ell(z_l, {\color{red}{f_l}} (z_{l-1})) + c({\color{red}{f_l}}) = \ell_l + c_l.
$$

Note that the cost borne by each player depends only on the choices of its neighbors, which is why I call it "local".
Also, let's assume that our dataset is realizable, i.e. there exists a profile such that $\sum \ell_l < \infty$.

Is the Nash equilibrium of this game the optimum of $\mathcal F$?
Certainly not.
Consider $z$ such that $f_1(x) \neq z$, $f_2(z) \neq y$ for all $f_1\in \mathcal{H}_1$, $f_2 \in \mathcal{H}_2$.
Also, choose $f_1, f_2$ such that $y \neq f_2 \circ f_1 (x)$. Then the cost borne by each player is $\infty$, and none of them can deviate alone to reduce their cost.
This points to the necessity of allowing for deviations by multiple players at the same time, i.e. coalitions.

A strong Nash equilibrium is an equilibrium stable under deviations by any coalition of players. Would this be the optimum of $\mathcal F$?
No, because there might be two profiles $\sigma$ and $\sigma'$ with $c_1 > c_1'$, $c_2 < c_2'$, such that $c_1 + c_2 > c_1' + c_2'$, but $f_2$ wouldn't agree to deviate with $f_1$ to $\sigma'$ because it would increase its cost from $c_2$ to $c_2'$. In other words, the players might be "too selfish" to deviate to the optimum.

### Strong Nash with Transfers

Finally, we can consider a strong Nash equilibrium *with transfers*, where players can make binding agreements to pay for some costs of others in the coalition. Would this be an optimum of $\mathcal F$?
Yes.
The sum of the costs borne by all players $f_l$ and $z_l$ is exactly

$$
\sum_{l=1}^L c_l + 2 \ell_1 + 3 \sum_{l=2}^{L-1} \ell_l + 2 \ell_L
=
\sum_{l=1}^L c_l + \sum_{l=1}^L \ell_l
=
\mathcal F.
$$

A profile that's stable under the coalition of all players, with transfers, must be an optimum of $\mathcal F$ by definition.

<!-- Yes, because now if there is a profile $\sigma'$ with $\mathcal F' < \mathcal F$, then $\sum_l c_l' < \sum_l c_l$, so all players can coalesce to deviate to $\sigma'$ and share the cost reduction in such a way that each player is better off. -->

Conversely, is an optimum of $\mathcal F$ always a strong Nash equilibrium with transfers?
The answer turns out to be yes: consider a profile $\sigma$ that is an optimum of $\mathcal F$, and suppose for the sake of contradiction that there is a subset of players $ \\{ f_l, z_k \\} $ that can deviate to $ \\{ f_l', z_k' \\} $ such that the cost borne by this subset is reduced in sum. Since $\sigma$ is an optimum of $\mathcal F = \sum \ell_l + \sum c_l$, the cost borne by players outside the coalition must have increased by at least as much as the cost borne by the coalition has decreased.
(1)
Suppose a player $f_l$ outside the coalition had its cost $\ell_l + c_l$ increased due to the deviation. It's not possible for $c_l$ to increase since it only depends on $f_l$ which hasn't changed. So the increase in cost must come from $\ell_l$, which means that either $z_{l-1}$ or $z_l$ must have deviated. $\ell_l$ can only increase to $\infty$, so the deviation of $z_{l-1}$ or $z_l$ must have led to a cost of $\infty$ for the coalition, hence a contradiction that the coalition reduced its cost in sum.
(2)
Suppose a player $z_l$ outside the coalition had its cost $\ell_{l+1} + \ell_l$ increased due to the deviation. Since $z_l$ hasn't changed, the increase must come from either a change in $z_{l+1}$ or $z_{l-1}$. Again, the increase in $\ell_{l+1}$ or $\ell_l$ must be $\infty$, so the deviation of $z_{l+1}$ or $z_{l-1}$ must have led to a cost of $\infty$ for the coalition, hence a contradiction.


### Conclusion

That strong Nash with transfers implies optimality is trivial, but not the converse. Sure, the optimum would be stable under deviations by the coalition of *all* players. But it's not clear (and not always the case in general) that it's stable under a coalition by any *subset* of players. This was possible in our case because we used the zero-infinity loss, where an increase in cost of a player outside the coalition implies an increase in cost of at least one player inside the coalition, by $\infty$.

<!-- Essentially, we were looking at a constrained optimization problem,

$$
\min_{f_1, \dots, f_L} \sum_{l=1}^L c(f_l)
\quad \text{subject to }
f_L \circ f_{L-1} \circ \cdots \circ f_1 (x^{(i)}) = y^{(i)} \quad \forall i.
$$

The zero-infinity loss encodes the hard constraint, so that any coalition that violates the constraint would have to pay a cost of $\infty$. Allowing for transfers ensures that $\sum c(f_l)$ can be optimized globally, without falling into local optima. -->

A result I had initially hoped for was something more general, where $f_l$ are stochastic maps $Z_{l-1} \to \Delta Z_l$ and the loss $\ell$ is negative log-likelihood. This would be variational inference in a hierarchical latent variable model, with the approximate posterior $q(z)$ as a point estimate. Unfortunately, none of our ideas above work in this case.
This shouldn't be too surprising. The most natural framing of the problem is a fully cooperative game where all players share the same objective $\mathcal F$, but we were forcefully trying to turn it into a game where each player only bears a local cost.
