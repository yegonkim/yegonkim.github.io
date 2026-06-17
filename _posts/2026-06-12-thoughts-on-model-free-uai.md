---
title: Thoughts on Model-Free UAI
categories: ["Artificial Intelligence"]
date: 2026-06-12
math: true
published: true
---

Some further thoughts on my recent work "A Model-Free Universal AI".

***

To summarize, [my recent work](https://arxiv.org/abs/2602.23242) shows that Universal AI with Q-Induction (AIQI), essentially an on-policy distributional Monte Carlo control algorithm, is asymptotically $\varepsilon$-optimal in *general environments*. This resolves the question of whether there exists a *model-free* general RL algorithm.

### Initial Motivation: Policy Search

Thinking back, I was initially curious about whether a policy search algorithm, e.g. policy gradient, can be optimal in general environments, *without the help of any value functions*. I am now pretty much convinced that the answer is "no". Without having a world model with which we can roll out counterfactual trajectories at arbitary $t:t+T$ to evaluate alternate policies, the best we can do is to assume that a policy that did well at $t:t+T$ will do well at $t':t'+T$. But this assumption doesn't hold in general environments, due to non-stationarity. Moreover, we can't even evaluate whether the policy did "well" at $t:t+T$, without comparing against how other policies would have done at $t:t+T$.

Why was I curious about this? To be honest, I just *liked* (1) the theory of AIXI and (2) the idea of policy search algorithms. I liked them because: (1) AIXI doesn't make assumptions about environments in RL that I had beef with for a long time, and it's so much cleaner, mathematically, than other theories of RL, which to me is an indication that the theory is being applied at the right level; (2) policy search is the RL algorithm that best captures what I would call the "deep learning philosophy", that is, to optimize end-to-end what you *really want* to optimize (the policy), with as little hand-engineering as possible, e.g., world models, value functions, etc.
Moreover, processes at various scales can be regarded as policy search, from evolution to AI research.

### World models are more useful than I thought

However, little did I know back then that this philosophy applies only when the optimization target is stationary (or when we can *make* it stationary through counterfactual evaluations).
For instance, evolution can test out a bunch of organisms on essentially the same Earth, AI research can test out a bunch of AI algorithms on the same benchmarks, and policy gradient algorithms can test out a bunch of policy networks on the same environment.
In the terminology introduced in [this blog post](https://www.lesswrong.com/posts/ZDZmopKquzHYPRNxq/selection-vs-control), we can't turn a control problem into a selection problem without a world model.
After realizing that the hope for a general policy search algorithm was doomed, I turned to showing that a model-free algorithm can be optimal in general environments. I was inspired by Self-AIXI, a policy iteration algorithm that uses a world model for the policy evaluation step. With only an additional trick called periodic augmentation, a constant epsilon-greedy exploration, and some tedious analysis, it was possible to show that amortizing policy evaluation by learning from the history of ground truth returns is sufficient for optimality.

My initial conviction when starting this project was that future AI systems would have a world model obtained through self-supervised learning, whose (potentially goal-conditioned) predictions would be used as mere additional "features" by the policy network. Due to problems with naive policy search described above, I am now more open to the possibility that future AI systems will actually have explicit uses for the world model hard-coded *outside* the policy search module, and not just fed inside.

It's useful to view an action $a_i$ and percept $e_i$ in the AIXI literature as a policy, $\pi_{iT:i(T+1)}$ and the corresponding environmental feedback $e_{iT:i(T+1)}$, respectively.
This is closer to practice where we usually do not update our models at literally every time step.
An algorithm similar to AIQI would look something like this: at every time step $iT$ we have a world model that can ~reliably predict the next $T$ steps, and we also have a learned state value function that can predict the on-policy value of the end states $h_{<i(T+1)}$. We can then perform policy search inside the world model. We could even use the policy from the previous $T$-step iteration to warm-start the policy search.

### Personal Reflections

I learned a lot about RL from doing this work: the limitations of pure policy search algorithms, why value functions are natural things, on-policy vs off-policy algorithms, and the role of exploration.
It's funny that I couldn't get myself to read Sutton's *Reinforcement Learning* or implement RL algorithms, and that I had to take a (very) long detour through AIXI to start thinking about these basic concepts.
This reminds me again that I should spend less time banging my head against the wall trying to study something that doesn't really interest me.
Also, I had a lot of fun learning basic computability theory, probably more fun than I have had learning anything in a long time.

It's also cool that this work was noticed by people working on AIXI, and further discussions with Cole Wyeth led to a result on Self-AIXI which I previously thought was out of reach.
I'm also working now in [AIXI Labs](https://www.aixi.uk/) for a 3 months fellowship program, and the discussions there helped me choose my next research direction, which I was struggling with for months.
I should really just reach out and talk to people more, even if I don't feel ready for a discussion.

### Appendix A. Model-Free Policy Search

I claimed that we can't have a "pure" general policy search algorithm. However, we *can* construct something akin to a policy search algorithm with a value function, through a simple modification of AIQI. The modification is straightforward: you learn the Q-function like in AIQI, and at each step $t$, instead of following AIQI and taking the action that maximizes the Q-function, you learn a policy by doing supervised learning on the action that AIQI *would* have chosen at steps $<t$, and then take the action $a_t$ according to this learned policy. Although I haven't checked the details, I am pretty sure that this modified algorithm is also asymptotically $\varepsilon$-optimal in general environments.
This feels like a mere "distillation" of AIQI to a learned policy, which is true, but at a high level it shares the same spirit as actor-critic algorithms.

### Appendix B. Exploration

Although not meant to be practical in the first place, one can imagine how slow the convergence would be for Self-AIXI and AIQI with the dumb epsilon-greedy exploration.
A reasonable first attempt to speed up learning would be to adopt the KSA for exploration to quickly figure out which environment it is in (Self-AIXI), which policy it is (Self-AIXI), or which distributional action value function it has (AIQI).
Unfortunately, I've spent all day trying to do this (for one of my course final projects), and haven't been able to make it work.
I think the high level problem is that during the exploration phase we are directly intervening on the actions, and these don't tell us anything about Self-AIXI policy.
On a more technical level, the problem I faced was that there is no clean KSA-like objective on actions that satisfies both (1) the finite sum property, like KSA which is bounded above by $\text{Ent}(w)/w(\mu)$, and (2) the objective itself being an upper bound on the true environment/policy's KL divergence to mixture environment/policy.