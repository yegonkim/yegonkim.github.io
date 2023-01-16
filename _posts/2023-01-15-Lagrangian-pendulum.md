---
title: Understanding Lagrangian mechanics
categories: Physics
date: 2023-01-15
math: true
---

This is a post for rigor-obsessed people who, just like I did, had a hard time following the logic of Lagrangian mechanics, especially when applied to problems with reduced degrees of freedom such as the pendulum problem. This is not an introduction to Lagrangian mechanics, but rather a list of confusions I had when studying Lagrangian mechanics and how I resolved them.

TL;DR:
- Principle of *least* action is a misnomer, and it should be called principle of stationary action
- A stationary path in one coordinate system might not necessarily be stationary in another system

***

## Principle of *least* action?

The first misunderstanding I had when studying Lagrangian mechanics was that the path $$x$$ has to minimize the action

$$ A[x] = \int_a^b L(t,x,\dot x) dt $$

However, this doesn't hold true in the general case.
The path only needs to be stationary:

$$ \frac{d}{d\varepsilon} A[x+\varepsilon g] = 0 $$

for any $$ g $$ such that $$ g(a)=g(b) = 0$$. Roughly speaking, a change in the path $$ x $$ that maintains the same initial and final conditions ($$ x(a)$$ and $$x(b)$$) leads to **zero** first order change in the action. This is trivially satisfied if the path minimizes the action, but the converse might not hold true (e.g. if the path is a saddle point or a local maximum).

It is quite surprising that any stationary path satisfies the [Euler-Lagrange equation](https://en.wikipedia.org/wiki/Euler%E2%80%93Lagrange_equation#Statement), since it can be solved for knowing only the intial condition ($$ x(a)$$ and $$\dot x(a)$$). In other words, for every initial condition $$ x(a)$$ and $$\dot x(a)$$, there is a unique stationary path, and hence a unique destination $$x(b)$$. All other final conditions $$x(b)$$ will yield no solution to the Euler-Lagrange equation and hence no stationary path.

## Generalized coordinate system

The immediate application of Lagrangian mechanics is in solving problems with an alternate coordinate system. For instance, a weight suspended in a pendulum can be described with its $$x$$ and $$y$$ coordinates, but also with a single $$\theta $$ coordinate. It simply suffices to rewrite the Lagrangian in terms of $$\theta$$ and to solve for the Euler-Lagrange equation.

For this to make sense, we should verify that a stationary path in one coordinate system is stationary in the other and vice versa. This didn't seem obvious to me at first, and was the greatest cause of confusion. In fact, it's **not even true**.

### Stationarity is not preserved across different coordinate systems

Take the Lagrangian of a free particle with unit mass in two dimensions, for example.

$$ L(t, \mathbf{x}, \mathbf{\dot x}) = T-U = \frac12 (\dot x^2 + \dot y^2) $$

Now, take another coordinate system $$ \theta $$, say

$$ (x,y) = (\cos \theta,\sin \theta) $$

Then,

$$ L(t, \theta, \dot \theta) = \frac12 ((-\sin\theta)^2 + (\cos \theta)^2 ) = \frac12 $$

We can easily see that the path

$$ \theta (t) = t $$

satisfies the Euler-Lagrange equation and is thus stationary in the $$ \theta $$ coordinate system. However, this path is

$$ (x,y) = (\cos t, \sin t) $$

in the first coordinate system, which doesn't satisfy the Euler-Lagrange equation. This effectively shows that a stationary path in one coordinate system might not be stationary in another coordinate system.

### Pendulum problem

With all this in mind, how can we rigorize the solution to the pendulum problem with Lagrangian mechanics in $$ \theta $$ coordinate system? Let's say the pendulum has unit radius and unit mass. Also, let $$ M_\theta$$ denote the manifold parametrized by $$\theta$$, that is, a circle of radius 1.

The **first** hurdle arrives when we try to formulate the problem in x-y coordinates: what is the potential $$ U(x,y) $$? This question is ill-posed, since we haven't specified how the pendulum behaves when the weight is not in $$ M_\theta$$. However, we can make the assumption that there does exist a setting where the force is conservative and it behaves just as we want when the weight is in $$M_\theta$$. When the weight is on such a path, the only force it receives is the tension in the string and the gravitational force. By evaluating the line integral of force over displacement along some path in $$M_\theta$$, we can derive that the potential difference between two points in $$M_\theta$$ is exactly the difference in their gravitational potential energy (since displacement is perpendicular to the tension).

By letting $$ U(1,0) = 0 $$, we obtain

$$ U(x(\theta), y(\theta)) = gy = -g\sin \theta $$

Therefore, the Lagrangian of the system in $$ M_\theta$$ is

$$ L(t,\theta,\dot\theta) = T - U = \frac12 \dot\theta^2 + g\sin \theta $$

The **second** hurdle is in the fact that a stationary path in $$\theta$$ coordinate might not necessarily be a stationary path in the x-y coordinates. This is because the set of perturbations in $$\theta$$ coordinate is a subset of perturbations in the x-y coordinates. There might be a perturbation in the ($$\theta$$-)stationary path outside $$M_\theta$$ that leads to a non-zero first order change in the action, rendering the path non-stationary in x-y coordinates. This is exactly what happended in the example given in the section above. On the other hand, a stationary path in the x-y coordinates must be stationary in the $$\theta$$ coordinate, because all perturbations in $$\theta$$ coordinate are also perturbations in the x-y coordinates.

What we need is an additional assumption that, given certain initial conditions ($$\mathbf{x}(a)$$ and $$\mathbf{\dot x}(a)$$), the stationary path in the x-y coordinates lies entirely within $$M_\theta$$. This path is also stationary in the $$ \theta $$ coordinate, and thus a solution to the Euler-Lagrange equation with respect to $$\theta$$. This key assumption is reasonable in the case of a pendulum, since we already know that with certain initial conditions, the weight travels along a path lying entirely within $$ M_\theta$$. In the pathological example of a free particle given above, such an assumption cannot hold true. We know that whatever the initial conditions are, the free particle won't move in a circular motion.

