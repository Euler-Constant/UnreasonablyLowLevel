# Day 12-13: Superword Level Parallelism (so help me God)

- Paper: "Exploiting Superword Level Parallelism
with Multimedia Instruction Sets", Larsen-Amarasinghe, 2000
- Date: 2026-05-02
- Status: Still understanding this.

## First impressions: 
I initially wanted to do it for the sake of doing a compiler related case study, but I was actually drawn to vectorization techniques a lot more (mostly because I personally do not like the idea and concept of autovec, so I wanted to check through it properly). From there, I got interested in specific implementations, and I liked the goSLP implementation I actually found, which led me to its origins, Superword Level Parallelism.

## Core Breakdown:
I feel like I shouldn't be doing it, because I'm **still** trying to wrap my head around it, but what it is supposed to be short(er) SIMD parallelisms arising from "independent isomorphic statements". Now my Abstract algebra classes came in realllll handy here, and it kind of has the same roots, sort of; Two statements are said to be isomorphic if they perform the same operations in the same order. The foundational concept is statement packing, replacing a group of isomorphic statements with a single SIMD instruction that operates on their operands packed together.

The hierarchy seems really straightforward; vector parallelism is a subset of both Loop-level parallelism and Superword-level parallelism, and ILP (instruction-Level Parallelism) as a precept of SLP. In reality, ILP is really just a restricted form of SLP.

The takeaway from the paper most of the time is that loop unrolling transforms vector parallelism into SLP.  This means their basic-block-level analysis can subsume what vectorizing compilers do, without needing the complex loop transformation machinery. Vector parallelism really is just a special case of SLP where you're packing unrolled copies of the same original statement.
