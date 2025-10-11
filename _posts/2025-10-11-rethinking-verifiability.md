---
title: Rethinking Verifiability
categories: Artificial Intelligence
date: 2025-10-11
math: true
published: true
---

I've been recently thinking about [verifiable problems](https://www.jasonwei.net/blog/asymmetry-of-verification-and-verifiers-law), and the different ways in which they manifest:
1. The problem instance we want to solve is verifiable, e.g. a coding task with self-contained test cases.
2. There's a dataset of related problems for which solutions are verifiable with the help of privileged information, e.g. competition math problems with access to ground truth answer.

[AlphaEvolve](https://arxiv.org/abs/2506.13131) is one method suited for the first scenario. It evolves solutions with mutations that are guided by LLMs and scored with the verifiers. [RLVR](https://arxiv.org/abs/2501.12948) is a method suited for the second scenario. It uses privileged information at training time to verify solutions to training instances, and the goal is to generalize to unseen test instances without access to privileged information.

It occurred to me that these two scenarios are quite different, yet this difference doesn't seem to be widely talked about. The [idea](https://x.com/SimonsInstitute/status/1839354223521374581?lang=en) that RLVR trained models are performing "search" in a chain-of-thought might exacerbate the confusion. This idea is referring to search against a learned, implicit "verifier" that operates on the test problem instance during chain-of-thought. This is quite different from the verifier that RLVR is trained with, which is based on privileged information that is not available at test time.

It should also be noted that the two scenarios are not mutually exclusive. One might have a dataset of verifiable problems with privileged information, and also want to solve a new problem instance that is verifiable, perhaps with weaker verification due to less information.<sup>1</sup> To clarify, by "weak", we mean that it's farther away from the ground truth verification. For example, we might have self-contained test cases for coding problems in the training dataset, but this might not be the case for the problem at test time where we might want to infer test cases from the natural language prompt, setting up a weak verification scheme.
Note that even verification with privileged information at training time might not be the same as ground truth verification. The problem of strengthening verification, whether at training time or at test time, is commonly referred to as [scalable oversight](https://www.lesswrong.com/posts/hw2tGSsvLLyjFoLFS/scalable-oversight-and-weak-to-strong-generalization).

Finally, we can unify the two scenarios by thinking of learning as *search over programs*. Just as the first scenario is search for a solution that passes the verifier, the second scenario can be regarded as search for a program that, when applied to problems on the training dataset, yields solutions that all pass the verifier. In other words, both scenarios are essentially search tasks equipped with verifiers. Moreover, in the first and second scenario combined, i.e. when we have both a training dataset and a test time verification scheme, the learned program might make use of this verification scheme at runtime, whether we steered it to do so, or it learnt to do that by itself on the training dataset. A naive example that I suspect will proliferate within the next year or two, is the **addition of evolutionary search like AlphaEvolve to the toolbox of LLM agents**. This would prove especially advantageous in many practical scenarios, where not only the problem instance is verifiable to a weak degree, but also can be divided into subproblems that are verifiable to a stronger degree. In such scenarios, the LLM agent could dynamically create appropriate verification schemes and make use of evolutionary search to solve the subproblems.

[1] An interesting example of the two scenarios combined is the [ARC-AGI benchmark](https://arcprize.org/arc-agi/2/). For each problem, one can search for a program that solves the example cases, and apply the program to the test cases. Solving the example cases is a "weak" verification of the program, because there might be multiple programs that solve the example cases, and therefore the program is not guaranteed to solve the test cases.
