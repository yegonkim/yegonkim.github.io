---
title: Alignment Research Accelerates AI Progress
categories: Artificial Intelligence
date: 2025-10-31
math: true
published: true
---

Alignment research, especially research on practical alignment methods for current AI systems, contributes significantly to AI progress, [[1]](https://openai.com/index/chain-of-thought-monitoring/) and will do so even more in the coming future.
For instance, alignment is a major obstacle to building automated researchers, since evaluating research is a hard task and there are all kinds of ways  AIs could perform adversarial optimization against our evaluation schemes.
Automated AI researchers will greatly accelerate AI progress, and *could* lead to a fast takeoff, [[2]](https://www.forethought.org/research/will-ai-r-and-d-automation-cause-a-software-intelligence-explosion) posing risks of developing misaligned AI systems.

I find it unlikely that the first line of automated researchers will be so capable that failing to align them will pose serious risks. Instead, they will fail to achieve our goals in visible ways, e.g. trying to deceive the evaluator but being caught relatively easily, and will simply be deemed useless.
If the alignment of automated AI researchers is *so hard* that we can't solve it for a long time, then we could get more time to make societal preparations for AGI, and to research alignment in a way that's transferable to future systems. One promising direction is to create, in advance, general alignment benchmarks, which will serve as both a target for optimization and a way to measure progress in the future. When automated researchers arrive, they can be used to optimize the alignment benchmarks in the same way they are used to optimize for capabilities. The amount of resources we invest into researching alignment would depend on our estimate of how much is needed to achieve sufficient alignment, which would also be crucially informed by results on the alignment benchmarks that measure progress.

Even if you don't want to accelerate current AI progress because it might lead to an unprepared fast takeoff, some arguments *for* researching alignment of current AI systems include:
- It might help us with alignment for future systems, whether by direct transfer, or by providing insights. This would depend heavily on how similar the AI systems created by automated researchers will be to the current systems. However, if the future research loop is much more powerful than the current AI research loop, then current progress on alignment doesn't seem to matter much, i.e. "one year of progress on alignment today will be achieved in one day in the future".
- Perhaps more importantly, researching practical alignment can provide unexpected insights about how we should *evaluate* alignment, which informs the design of general alignment benchmarks.
- Certain methods might turn out to be so expensive or not strong enough in practice that it's unlikely to contribute significantly to current AI progress, but whose experiments with current systems can still provide useful insights for future alignment research. 

Some concerning possibilities for the approach of creating general alignment benchmarks include:
- It might be too hard to *create* such benchmarks, perhaps almost as hard as aligning the AI system at hand. For instance, consider a naive benchmark where we give coding problems and check if the AI system solves them without cheating. But how do we check if the AI is cheating? This is basically the alignment problem itself. One potential remedy would be to create problems that are intentionally injected with vulnerabilities that the AI system could exploit, and check if the AI system finds and exploits them. But this is quite limited in scope. To test superhuman deception capabilities, we would need to create superhumanly complex vulnerabilities, which again seems very difficult.
- *Optimizing* against such benchmarks might be so hard, that we need an AI system with capability $n+1$ to find successful alignment methods for an AI system with capability $n$. If this is the case, then we can't trust that the automated researchers are performing research without deception, or other dangerous activities.

Much of the research on scalable alignment, especially scalable oversight, already are targeted towards evaluating alignment methods in a scalable manner. [[1]](https://arxiv.org/abs/2211.03540) We should continue to prioritize this line of research, as it seems crucial for scalably tackling alignment with future automated researchers.

