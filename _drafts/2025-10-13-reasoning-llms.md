---
title: One Who Makes Evals Controls the World
categories: Artificial Intelligence
date: 2025-10-14
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
credit assignment -> atari games
exploration -> Montezuma's Revenge


gdpval

i think some of these mistaken for 


in general we can use eval for two purposes:
- evaluating the progress of current AI in what the eval is trying to measure
- optimizing the AI system against the eval, to make progress

due to goodhart's law, we probably shouldn't be using the same eval for both purposes. fortunately, 

but overfitting to synthetic tasks can be problematic. for the final optimization we would probably need to optimize against realistic tasks, by using novel ideas proposed during optimization against the extreme cases in synthetic tasks.

another thing to note is that many of the historical "solutions" to these benchmarks involved using existing ideas and composing them, with some slight tweaks here and there. the usefulness of benchmarks is not only in eliciting new ideas, but also in picking out the ideas that actually work when tweaked ans scaled the right way.


the most valuable researchers will be those who have a good grasp of what's missing in the current AI systems, and propose relevant tasks that actually require novel ideas to solve. 


can we "meta"-optimize the agent that performs the optimization against these benchmarks?
it seems quite clear to me that certain general abilties, like long term memory, sample efficiency, world modeling, and planning, will lead to direct improvements in this "meta"-skill. that's why it's the most productive to think of abilities that are so general that optimizing for it leads to improvements in the ability to optimize itself.
for instance, according to ablation, alphaevolve benefits a lot from good initial idea. retrieving the good initial ideas from the large pile of existing ideas would be a helpful skill.

how can llms organize their research? academic papers might not be the optimal choice. we might have 

therefore, i imagine that 



<!-- if there's one thing i learnt from listening to a bunch of neuroscience courses in undergrad, it woudl be that the brain  -->

<!-- i think overall, what our current ai systems might be lacking the most is effective  -->




<!-- we probably need some pretty complicated and modular architecture to reach human level intelligence. this doesn't defy the bitter lesson. these benchmarks provide data, and we are optimizing against it by scaling compute and *ideas*. -->


<!-- on the bright side, this will probably allow us (humans) to gain more insight about intelligent systems and help us with alignemnt. it's also possibly more interpretable when the system is modular. -->
