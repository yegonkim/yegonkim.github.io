---
title: When Analysis by Synthesis Fails (and When It Succeeds)
categories: Artificial Intelligence
date: 2025-12-09
math: true
published: true
---

Analysis by synthesis is an idea originating from Helmholtz, that the brain has an internal generative model of the world, say $P(x \mid y)$, and perception is the process of finding an explanation $y$ for the observed data $x$ by inverting this model, i.e., finding the $y$ that maximizes $P(y \mid x)$. Thus one is "analyzing" the data $x$ by "synthesizing" it from the model. The most straightforward approach to unsupervised learning would thus be to learn a generative model of the 



the problem is we only have few tasks that matter to us, and a bunch of raw data/tasks that we don't know the importance of. fundamentally, one needs to find a way to make use of the raw data in a way that helps us generalize to all tasks that matter to us. 1. through better algorithms e.g. simclr or 2. by framing this itself as a deep learning task. one promising approach is to use memory-augmentation, where the model learns to retrieve some bits of the raw data, and use them to help solve the task at hand. that is, the model learns turns each task into a meta-learning task, where the raw data is used as inner training data for the meta-learning. 
approach 1 certainly holds an advantage in finding extremely simple algorithms that can't reliably be learned through deep learning, but approach 2 seems like what's missing in current deep learning approaches: making use of unsupervised data in a very complex, flexible way, e.g. reusing abstractions that were 

X lowers kolmogorov complexity of Y|X by allowing simple programs to extract bits from X that are useful for Y. in other words, both X AND the extracted bits from X can be used on and on in solving tasks in Y. 
actually, this is also relevant to the current qwen3 models that throw out the reasoning after taking actions in multi turn settings. it can see X inside the task, but not the extracted bits from X.
this would also be a problem for the idea of training models on NTP with chain-of-thought, since the model can't reuse the chain-of-thought from predicting previous tokens.
perhaps the way forward is learning a way to compress the chain-of-thought into a continuous latent thought. so it just becomes a subproblem of compressing long contexts into a few tokens, like Titan. 


so the lesson i guess is that we should definitely be doing both 1. improving simple unsupervised learning algorithms and scaling raw data (one promising approach would be active data sampling for next token prediction) but also, 2. scaling tasks that we actually care about, including the long horizon ones (it's the environment that matters, and not the reward) and improve unsupervised learning algorithms such that we are no longer bottlenecked by the sparsity of reward, by learning a lot from the observations during interactions with the environment.

hm.. but if we are only reinforcing the deep learning network (that retrieves and uses memory) based on the reward signal, then we are still bottlenecked by the sparsity of reward. how do humans solve it? do they have a very smart unsupervised learning algorithm that works across the board? what would that be?

should the memory only be retrieved within-task, or across the entire history of training? the first relieves the burden of memorizing a LOT, and the latter allows for less burden on the designers of environments to come up with tasks that require long-term dependencies.  eh but i might be overestimating the cost of the latter, since one can come up with pretty hard tasks in simple environments like gridworlds or coding. 

the real conceptual test would be whether the system at hand can meta-learn things like bellman updates or model-based planning algorithms. 


1 bit information of reasoning models


is there something about rlvr that allows it to generalize better than backprop e.g. with adaptive compute time ? 

it seems that humans apply quite some heavy processing of its raw data (e.g. when you study something) which must have been learned. so it seems that we do extensively use across-task transfer of abstractions. but how do we learn to create such abstractions in the first place? i think it's possible that 



there must definitely be some algorithms, e.g. dreamer, planning, self-supervised learning, consistency of knowledge, etc. that help in overcoming the sparsity of reward problem. but i think an additional key is to find algorithms that can work across a wide variety of tasks/environments, so that we don't have to hand-design specific algorithms for each environment.

