---
title: Understanding Lagrangian mechanics
categories: Physics
date: 2023-01-15
math: true
---

This is a post for rigor-obsessed people who, just like I did, had a hard time following the logic of Lagrangian mechanics, especially when applied to problems with reduced degrees of freedom such as the pendulum problem. This is not an introduction to Lagriangian mechanics, but a list of confusions I had when studying Lagrangian mechanics and how I resolved them.

***

## Principle of *least* action?

The first misunderstanding I had when studying Lagrangian mechanics was that the path $$x$$ has to minimize the action

$$ A[x] = \int_a^b L(t,x,\dot x) dt $$

However, this doesn't hold true in the general case.
The path only needs to be stationary:

$$ \frac{d}{d\varepsilon} A[x+\varepsilon g] = 0 $$

for any $$ g $$ such that $$ g(a)=g(b) = 0$$. Roughly speaking, a first order change in the path $$ x $$ that maintains the same initial and final conditions ($$ x(a)$$ and $$x(b)$$) leads to **zero** first order change in the action. This is trivially satisfied if the path minimizes the action, but the converse might not hold true (e.g. if the path is a saddle point or a local maximum).

It is rather surprising that any stationary path satisfies the [Euler-Lagrange equation](https://en.wikipedia.org/wiki/Euler%E2%80%93Lagrange_equation#Statement), since it can be solved for knowing only the intial condition ($$ x(a)$$ and $$\dot x(a)$$). In other words, for every initial condition $$ x(a)$$ and $$\dot x(a)$$, there is a unique stationary path, and hence a unique destination $$x(b)$$. All other final conditions $$x(b)$$ will yield no solution to the Euler-Lagrange equation and hence no stationary path.

## Reduced degrees of freedom

The immediate application of Lagrangian mechanics is in solving problems with an alternate coordinate system. For instance, a weight suspended in a pendulum can be described by the $$x$$ and $$y$$ coordinates, but also by a single $$\theta $$ coordinate. It simply suffices to rewrite the Lagrangian in terms of $$\theta$$ and to solve for the Euler-Lagrange equation.










## A brief recap of Lagrangian mechanics

### Euler-Lagrange equation

Suppose there is a function $$ L(t,x,v) $$ where the variables can be interpreted as time, position, and velocity, respectively. For a path $$ q(t) $$, let the action be

$$ A_{a,b}[q] = \int_a^b L(t,q(t),\dot q(t)) dt $$

where $$ \dot q = \frac{dq}{dt} $$. If the path satisfies the , its action is minimized.



From the equation of motion

$$ F=ma $$

,

$$ -\frac{\partial U}{\partial x} = \frac{d}{dt} (m \dot x) $$

where the force $$ F $$ is [conservative](https://en.wikipedia.org/wiki/Conservative_force#Mathematical_description) with potential $$ U $$ and $$ \dot x = \frac{dx}{dt} $$.

Now, we define the function

$$ L(t, x, \dot x) = \frac12 m \dot x^2 - U(t, x) $$

We can easily see that the equation of motion is equivalent to

$$ \frac{\partial L}{\partial x} = \frac{d}{dt} \frac {\partial L}{\partial \dot x} $$

(Note that we have abused the notation a bit, since in the definition of the function $$ L$$, $$\dot x$$ is **not** $$\frac{dx}{dt}$$, but an independent variable on its own. On the other hand, when we evaluate $$L$$ or its derivatives, we always plug in $$\frac{dx}{dt}$$ into $$\dot x$$.)

But this is exactly the . This means that for any path $$ x $$ that satisfies the equation of motion is **stationary**: 













