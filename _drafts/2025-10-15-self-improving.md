---
title: 
categories: Artificial Intelligence
date: 2025-10-15
math: true
published: false
---

analyze actual outputs of alphaevolve.
how creative it actually is now, and how better it will probably get.


visual recognition -> cifar10
algorithm learning and execution (learning with/without execution trace)-> math competitions 
long term memory
compositional generaliaiton -> scan
length generalization -> parity
perfect information two player zero sum game -> Go
sample efficiency -> arc
exploration -> Montezuma's Revenge
planning -> 
image understanding -> 

long-term credit assignment

i think some of these mistaken for evaluating other capabilities, e.g. maths competition for general reasoning.


in general we can use eval for two purposes:
- evaluating the progress of current AI in what the eval is trying to measure (e.g. gdpval)
- optimizing the AI system against the eval, to make progress

due to goodhart's law, we probably shouldn't be using the same eval for both purposes. fortunately, 

but overfitting to synthetic tasks can be problematic. for the final optimization we would probably need to optimize against realistic tasks, by using novel ideas proposed during optimization against the extreme cases in synthetic tasks.

another thing to note is that many of the historical "solutions" to these benchmarks involved using existing ideas and composing them, with some slight tweaks here and there. the usefulness of benchmarks is not only in eliciting new ideas, but also in picking out the ideas that actually work when tweaked ans scaled the right way.

this conforms to the bitter lesson. we should scale up the search for solution, whether it's sgd or evolutionary search of architectures, or even the training data. and of course, we should scale the learning of the system that does the search.

the most valuable researchers will be those who have a good grasp of what's missing in the current AI systems, and propose relevant tasks that actually require novel ideas to solve. 


can we "meta"-optimize the agent that performs the optimization against these benchmarks?
it seems quite clear to me that certain general abilties, like long term memory, sample efficiency, world modeling, and planning, will lead to direct improvements in this "meta"-skill. that's why it's the most productive to think of abilities that are so general that optimizing for it leads to improvements in the ability to optimize itself.
for instance, according to ablation, alphaevolve benefits a lot from good initial idea. retrieving the good initial ideas from the large pile of existing ideas would be a helpful skill.

how can llms organize their research? academic papers might not be the optimal choice. we might have some sort of "research logs" that are more structured and easier to parse.


I find it hard to believe that evolution had high selective pressure for extremely long horizon tasks, such as planning for months. 


how is it better than human researchers?
- can run in parallel (many ais)
- clear reproducibility and hence better credit assignment
- faster iteration (time to code, run scripts, job scheduling, etc.)
- better structured knowledge of other agents' research and hence less time spent on reading papers
- more effective collaboration (e.g. through shared codebases, tools, and environments)
- no forgetting over time


benchmarks that evaluate ai progress will require a lot of resources, possibly a community collaboration.

researchers will become benchmark creators, and the best researchers will be those who can create the most challenging benchmarks that require novel ideas to solve. and the skill of optimizing against these benchmarks will be less important — any ai will be able to do that. this doesn't mean that insight on optimization is no longer needed. i believe that humans will still be better than ai at deep insights about why certain ideas work, for the forseeable future. (based on extrapolation of past ais abilities to build )
i.e. people who actually know what they're doing.

i think this might be somewhat hindered by centralization of ai resources to researchers who were good at optimizing.

in general, solving a problem will involve humans who are good at specifying the problem in such a manner that a solution optimized by an ai will actually solve the problem in the real world. this is a skill in itself that will be very valuable for a long time, as it requires deep understanding of the problem domain, and an intuition about the capabilities of the ai systems.
traditionally, the role of benchmark creation was done by a few humans, 
there are many problems in the world, so many that there will never be a shortage of them. with the advent of optimizing ai systems, the bottleneck will shift to the quality and quantity of humans who can specify the problems in a way that ai systems can solve. it's funny, because creating benchmarks is often seen as a menial task, and somewhat looked upon by some immature researchers.
and make no mistake, this doesn't mean that academic papers will just become proposals for benchmarks. that would just be slop. valuable papers will be those that propose benchmarks to optimize against, and actually show that optimizing against them can produce solutions that are favorable in terms of the test benchmarks.
however, a concern is that the researchers will use the ai optimizers to optimize against the test benchmarks (which they most do already although through human optimization), 

i wouldn't call myself a very "imaginative" kind. it's hard for me to assign non-zero probability to events that i can't imagine how they would happen. alphaevolve was the trigger that made me think seriously about significant acceleration of research, and including ai research. 


