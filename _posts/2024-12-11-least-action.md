---
title: D'Alembert's Principle and the Principle of Least Action
categories: Physics
date: 2024-12-12
math: true
published: true
---

One of the most famous examples for introducing Lagrangian mechanics is deriving the dynamics of a pendulum. I was never taught d'Alembert's principle, which is a crucial part of the derivation.

TL;DR:
- D'Alembert's principle provides the justification for using Lagrangian in a constrained system

***

Remember how the Lagrangian is derived? We start with the Newtonian equations of motion, and equate it with the Euler-Lagrange equation

$$ \frac{\partial L}{\partial q} - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} = 0 $$

to derive the principle of least action.

$$ \delta A =  \delta \int_{t_1}^{t_2} L(q, \dot{q}, t) dt = \int_{t_1}^{t_2} \left( \frac{\partial L}{\partial q} - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \right) \delta q dt = 0 $$

The usual derivation of the equation of motion for a pendulum goes like this: calculate the kinetic and potential energy, write down the Lagrangian, and then apply the Euler-Lagrange equation. There was something that deeply bothered me about this derivation. The Lagrangian is derived from the Newtonian equations of motion, which applies to unconstrained systems, and yet we are applying it to a constrained system without any justification.

This is where d'Alembert's principle comes in. Say $C_i$ and $F_i$ are the constraint force and applied force on the $i$-th particle, respectively, and $F^{(T)}_i = F_i + C_i$ is the total force acting on the $i$-th particle. From Newton's second law, we have

$$ F^{(T)}_i - \dot p_i = 0 $$

where $p_i$ is the momentum of the $i$-th particle. Now, project them onto *virtual displacements* $\delta q_i$ that satisfy the constraint of the system.

$$ \sum_i F^{(T)}_i \cdot \delta q_i - \sum_i\dot p_i \cdot \delta q_i = 0. $$

It's usually the case that the virtual work done by the constraint force is zero.

$$ \sum_i C_i \cdot \delta q_i = 0 $$

This is, for instance, true for tension in a rod, or normal force in a surface. One can verify this with some vector analysis on the forces and constraint-satisfying displacements.
Finally, we obtain **d'Alembert's principle**

$$ \sum_i F^{(T)}_i \cdot \delta q_i - \sum_i\dot p_i \cdot \delta q_i=\sum_i F_i \cdot \delta q_i + \cancel{ \sum_i C_i \cdot \delta q_i } - \sum_i\dot p_i \cdot \delta q_i = 0. $$

$$\sum_i F_i \cdot \delta q_i - \sum_i\dot p_i \cdot \delta q_i = 0$$

or for a one-dimensional one-particle system,

$$ F \delta q - \dot p \delta q = 0 $$

Why did we do this? Because we can now apply this reasoning to the principle of least action. If there exists an appropriate Lagrangian $L$ that satisfies

$$ \left( \frac{\partial L}{\partial q} - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \right) \delta q = (F - \dot p) \delta q $$

for every virtual displacement $\delta q$ that satisfies the constraint of the system, then we can write

$$ \delta A = \int_{t_1}^{t_2} \left( \frac{\partial L}{\partial q} - \frac{d}{dt} \frac{\partial L}{\partial \dot{q}} \right) \delta q dt = 0 $$

where $\delta q$ satisfies the constraint of the system.

Essentially, one needs to think of a constrained system as a system defined on a manifold, and the constraint is the manifold itself. The virtual displacement is then a tangent vector, and Lagrangian mechanics becomes calculus of variations on the manifold. It's also worth noting that d'Alembert's principle can be used to derive the equation of motion for a pendulum without using the Lagrangian, and I think it's a nice exercise to try before learning about the Lagrangian method.