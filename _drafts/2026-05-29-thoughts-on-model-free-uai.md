---
title: Thoughts on Model-Free UAI
categories: Artificial Intelligence
date: 2026-05-29
math: true
published: true
---

Some further thoughts on my recent work "A Model-Free Universal AI".

***

To summarize, [my recent work](https://arxiv.org/abs/2602.23242) shows that Universal AI with Q-Induction (AIQI), essentially an on-policy distributional Monte Carlo control algorithm, is asymptotically $\varepsilon$-optimal in *general environments*. This resolves the question of whether there exists a *model-free* general RL algorithm, with a "yes".

### Initial Motivation

Thinking back, I was initially curious about whether a policy search algorithm, e.g. policy gradient, can be optimal in general environments, *without the help of any value functions*. I am now pretty much convinced that the answer is "no". Without having a world model with which we can roll out counterfactual trajectories at arbitary $t:t+T$ to evaluate alternate policies, the best we can do is to assume that a policy that did well at $t:t+T$ will do well at $t':t'+T$. But this assumption doesn't hold in general environments, due to non-stationarity. Moreover, we can't even evaluate whether the policy did "well" at $t:t+T$, without comparing against how other policies would have done at $t:t+T$.

Why was I curious about this? To be completely honest, I just *liked* 1) the theory of AIXI and 2) the idea of policy search algorithms. I can roughly speak of the reasons why I liked them: 1) AIXI doesn't make assumptions about environments in RL that I had beef with for a long time, and it's so much cleaner, mathematically, than other theories of RL, which to me is an indication that the theory is being applied at the right level; 2) policy search is the RL algorithm that best captures what I would call the "deep learning philosophy", that is, to optimize end-to-end what you *really want* to optimize (the policy), with as little hand-engineering as possible, e.g., world models, value functions, etc. Little did I know back then that this philosophy applies only when the optimization target is stationary (or when we can *make* it stationary through counterfactual evaluations).

After realizing that the hope for a general policy search algorithm was doomed, I turned to showing that a model-free algorithm can be optimal in general environments. I was inspired by Self-AIXI, a policy iteration algorithm that uses a world model for the policy evaluation step. With only an additional trick called periodic augmentation, a constant epsilon-greedy exploration, and some tedious analysis, it was possible to show that amortizing policy evaluation by learning from the history of ground truth returns is sufficient for optimality.
<!-- Something that I strived for is the minimality of assumptions required for the proof, unlike the original Self-AIXI paper which makes one assumption after another. I think I fully succeeded in this, and the final proof requires no more assumptions than the standard proofs in the AIXI literature. -->

### Model-Free Policy Search

Although we likely can't get rid of both world models and value functions for general policy search, I think we *can* construct a policy search algorithm *with a value function*, through a simple modification of AIQI. The modification is straightforward: you learn the Q-function like in AIQI, and at each step $t$, instead of following AIQI and taking the action that maximizes the Q-function, you learn a policy by doing supervised learning on the action that AIQI *would* have chosen at steps $<t$, and then take the action $a_t$ according to this learned policy. Although I haven't checked the details, I am pretty sure that this modified algorithm is also asymptotically $\varepsilon$-optimal in general environments.
This feels like a mere "distillation" of AIQI to a learned policy, which is true, but at a high level it shares the same spirit as actor-critic algorithms.

### How my views on future AI systems have changed


but we will likely do policy search inside a world model, with the learned value of the final state as the target. policy search inside a world model is, in the limit, essentially the same as planning inside a world model, but it can be more efficient in practice, and it also allows us to do things like regularization towards some base policy.
more concretely, I tend to view each action $a_i$ and percept $e_i$ in the general environment formulation as a $T$-step policy $\pi\_i$ and environmental feedback $\nu^{\pi_i} (e\_{iT:i(T+1)} \mid h\_{<t})$.



just easier to understand rl for me, and likely for some other people too, especially people who are not comfortable with standard assumptions in RL



### Application to Self-AIXI

Another key contribution, which I only discovered after rounds of discussion with Cole Wyeth, is that the proof techniques for AIQI transfer to Self-AIXI, allowing for a proof of the asymptotic optimality of Self-AIXI without ad-hoc assumptions. In fact, the problem with the dubious assumptions in the original proof for Self-AIXI had been [noted a year ago](https://www.lesswrong.com/posts/B6gumHyuxzR5yn5tH/unbounded-embedded-agency-aedt-w-r-t-rosi) by Cole.
It's funny that 4 months ago I was so furious that no one seemed to have noticed the problem, and it turns out I just didn't look hard enough on LessWrong.





epsilon exploration -> i was just trying to prove the damn theorem
turns out it's been proposed as a simple solution to action counterfactuals


### Possible Improvements to Exploration

speed of convergence
altohugh not meant to be practical in the first place, one can imagine how slow AIQI, due to its dumb epsilon-greedy exploration. one can apply ksa to speed up learning.
straightforward for self-aixi (KL of xi^zeta and nu^pi)
not so straightforward for AIQI.



### Personal Reflections



I learned a lot about RL from doing this work: the limitations of pure policy search algorithms, why value functions are needed, the intuition that policy iteration vs. planning is a fundamental distinction, perhaps even more so than model-free vs. model-based, and the importance/difficulty of safe exploration.
I feel like I can finally make sense of the standard RL literature. Things like on-policy vs off-policy RL, policy iteration, the role of value functions, etc. seemed completely arbitrary to me beforehand.
In hindsight, doing this work was just one of the *many* paths I could have taken to "make sense" of RL. For instance, a more ordinary path is to just read Sutton's *Reinforcement Learning* and tinker with actual RL algorithms on OpenAI Gym.

I think the path I took, which is to try to prove a theorem about a general policy search algorithm, is more in line with my personality and also more fun for me.
The problem is, there's nothing much I can do to "motivate" myself in 

quite a dillema, because one needs a certain amount of background knowledge to even understand some problems, while I just want to solve problems that no one has solved. I guess I just got lucky that the problem of model-free UAI was both unsolved and also something I could understand and be motivated to solve.
this also suggests that maybe it's a good thing that my taste in research is a bit "weird", because it allows me to find and solve problems that other people don't even see.


for the first time felt grateful to the field of AI, at least while doing research. 

