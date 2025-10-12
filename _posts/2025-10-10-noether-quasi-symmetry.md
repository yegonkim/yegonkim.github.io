---
title: Noether's Theorem and Quasi-Symmetry
categories: Physics
date: 2025-10-10
math: true
published: true
---

Noether's theorem is a fundamental result in physics that relates symmetries to conservation laws. The usual statement of Noether's theorem requires the action to be invariant under a continuous transformation. However, there are cases where the action is not strictly invariant but the dynamics remain unchanged. These cases are known as **quasi-symmetries**.

***

Consider a 1D constant force field on a single particle. The Lagrangian is given by:

$$ L = K - V = \frac{1}{2} \dot{q}^2 + q $$

(assuming unit mass and force for simplicity).

Here's a question to the reader: is this system invariant under spatial translation?
If you look at the Lagrangian, the answer is an obvious NO. $L$ depends explicitly on $q$. However, if you think of the system physically, no matter where you place the particle, it will experience the same dynamics. To see this more explicitly, consider the equation of motion for this system:

$$ \ddot{q} = 1 $$

Nowhere in this equation do we see the position $q$. The equation of motion is invariant under spatial translation.

This is actually a special case of when the Lagrangian changes by a total time derivative of some function $F(q,\dot q, t)$ under a transformation:

$$ L(q', \dot{q}', t) = L(q, \dot{q}, t) + \frac{dF}{dt} $$

This is called a **quasi-symmetry**. The reason why this is important is that the action integral changes by a boundary term:

$$ A' = \int_{t_1}^{t_2} L(q', \dot{q}', t) dt = \int_{t_1}^{t_2} \left( L(q, \dot{q}, t) + \frac{dF}{dt} \right) dt = A + F \big|_{t_1}^{t_2} $$

Since the variation of the action integral $\delta A$ is not affected by boundary terms, the equations of motion remain unchanged. In the case of our constant force field, we can see that under a spatial translation $q' = q + \nu $, the Lagrangian transforms as:

$$ L(q', \dot{q}', t) = \frac{1}{2} \dot{q}^2 + (q + \epsilon) = L(q, \dot{q}, t) + \epsilon = L(q, \dot{q}, t) + \frac{d}{dt}(\nu t) $$

where $F = \nu t$. Thus, the system exhibits a quasi-symmetry under spatial translation.

Noether's theorem states that for every continuous symmetry of the action, there is a corresponding conservation law. In the case of quasi-symmetries, the conserved quantity is modified by the function $F$. Specifically, suppose the infinitesimal transformation

$$ q'(t) = q(t) + \epsilon \delta q $$

leads to a change in the Lagrangian of the form

$$ L = L(q', \dot{q}', t) = L(q, \dot{q}, t) + \epsilon \frac{dF}{dt}. $$

Then we can write

$$ \frac{dL}{d\epsilon} = \frac{dF}{dt} $$

and also, as per the standard derivation of Noether's theorem,

$$ \frac{dL}{d\epsilon} = \frac{\partial L}{\partial q} \frac{dq}{d\epsilon} + \frac{\partial L}{\partial \dot{q}} \frac{d\dot{q}'}{d\epsilon} = \frac{\partial L}{\partial q} \delta q + \frac{\partial L}{\partial \dot{q}} \delta \dot{q} = \frac{d}{dt} \frac{\partial L}{\partial \dot q} \delta q + \frac{\partial L}{\partial \dot{q}} \delta \dot{q} = \frac{d}{dt} \left( \frac{\partial L}{\partial \dot{q}} \delta q \right) .$$

Equating these two expressions, we get

$$ \frac{d}{dt} \left( \frac{\partial L}{\partial \dot{q}} \delta q - F \right) = 0 $$

so that the quantity

$$ Q = \frac{\partial L}{\partial \dot{q}} \delta q - F $$

is conserved. In the case of our constant force field, the conserved quantity associated with spatial translation is

$$ Q = \frac{\partial L}{\partial \dot{q}} \cdot 1 - t = \dot{q} - t. $$



### Appendix

We know that a transformation that changes the Lagrangian by a total time derivative of some function $F(q,\dot q, t)$ leaves the equation of motion unchanged. But is the converse also true? A quick counterexample is the following:
Consider a free particle with Lagrangian

$$ L = \frac{1}{2} \dot{q}^2. $$

Now consider the transformation

$$ q'(t) = \lambda q(t) $$

where $\lambda \neq 1 $ is a constant scaling factor. The Lagrangian on the transformed trajectory is

$$ L(q', \dot q', t) = \frac{1}{2} \lambda^2 \dot{q}^2. $$

The equations of motion for the original and transformed systems are the same:

$$ \ddot{q} = 0,\qquad \ddot{q}' = 0 .$$

However, the Lagrangian does not change by a total time derivative of some function $F(q,\dot q, t)$ unless $\lambda = 1$.

References:
- Qmechanic. Answer to "Invariance of Lagrangian in Noether's theorem." Physics Stack Exchange, 8 Feb. 2012, physics.stackexchange.com/questions/11313/invariance-of-lagrangian-in-noethers-theorem/11317#11317.
- "Noether's Theorem." Wikipedia, Wikimedia Foundation, en.wikipedia.org/wiki/Noether%27s_theorem. Accessed 10 Oct. 2025.