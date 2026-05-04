# Day Fourteen: Today I tried looking at something completely new

- Paper: 
- Date: 2026-05-04
- Status: And now I'm completely confused

## Core Breakdown
It's been a rough past few days, but I've been trying to get around aware implementations of optimizations that **could be* used with SLP. Let me explain:

## The issue
Pass ordering in a compiler refers to the sequence in which different phases or passes of the compilation process are executed. The ordering is crucial as it affects how the source code is analyzed and transformed into executable code, ensuring that each pass has the necessary information from previous passes to perform its tasks effectively. 

Now, with this, the next thought should be that you'd be able to mix and match optimization techniques to get more efficient compilations. Which you should aim for, definitely. But the issue becomes that there are specific optimizations that just are not favourable with some, and it's especially blown out of proportions with Superword Level Parallelism.

So, the summarise the few cases, some passes are SLP-producers, meaning they should be run before SLP. Which is sort of a rarity because for reasons that would be explained in its algorithm (which I'd get to). Then consumers, destroyers and some are bidirectional. What I'm currently STRUGGLING with are the bi-directional passes, because they kind of require the pass from before to be SLP-aware, like knowing when to insert things like IR collapse or redundancy insertion, and it was just a fascinating thing I stumbled on.
