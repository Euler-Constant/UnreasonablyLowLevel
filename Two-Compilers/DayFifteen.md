# Day Fifteen: Going through (most of) the terminology in a bid to explain the algorithm

- Paper:
- Date: 2026-05-10
- Status: Paper analysis

## What exactly are we looking at?
Well, if you've been following this series, welcome back. We're still on Superword Level Parallelism, and to be completely honest, half my inactivity has been life throwing a very mean curveball at me, and the other half is just me not understanding the algorithm implementation properly, which is why I decided to go back to the base in a bid to explain how exactly all of this works, and in doing just that, it became really clear. I guess it was just a prerequisite gap xD

## So, back to base;

### What are compilers actually doing?
Specifically speaking, a compiler takes a program that conforms to a source grammar, which is the legal structure of the language, and produces a program that is legal according to the target grammar. These two programs are to be equivalent to one another (under the same ISA backend, of course)
Well, not quite, at least not in practice. See the thing is it does not go from source to destination just like that, they go through a series of Intermediate Representations (IRs), which are different formal languages with their own grammar and their own semantics, chosen to make a particular transformation easier. Different optimisation techniques use different IRs, like the three-address form which is explicitly stated in the paper to increase flexibility and code atomicity, making every single operation explicit, and every operation that would be worked on  would then be a visible, class-named statement. Others include the Single Static Assignment (SSA) format, although that would have to be a separate case study.

- Basic blocks
- Dependence 
- Isomorphism 

## The Algorithm itself;

### Loop Unrolling:
This is performed relatively early for reasons, as stated in the paper, is most easily done at a rather high level of the optimisation process. It is primarily used to turn vector parallelisms into basic blocks with SLP. In this technique, an unroll factor is used, as stated;

> "In order to ensure full optimisation of the superword datapath in the presence of a vectorizable loop, the unroll factor must be customised to the data sizes within the loop."

and is usually relative to the SIMD width (a bit of context I found scouring some more)

### Alignment Analysis:
Now this is just **a bit* nuanced, primarily because the reason we'd do this is not immediately obvious, which is for architectures that do not support unaligned memory accesses. This step helps to improve the performance gains by optimising with this particular technique. "Without it, memory accesses are assumed to be unaligned and the proper merging code must be emitted for every wide load and store"
SIMD 'load' instructions on most architectures require that the access itself is aligned to the SIMD width. Alignment analysis statically determines, for each load and store instruction, whether its address is guaranteed to be aligned at compile time. Basically, a type system. The result is an annotation on each load/store: either a known alignment value, or an unknown value (which has a symbol I cannot seem to find at the time of typing this).

### Pre-optimisation:
The IR is first flattened to three-address form, then classical optimisations are allowed to run after, some which could include;

- Constant and/or Copy propagation
- Dead Code Elimination
- Common Subexpression Elimination 
- Loop-Invariant Code Motion (Even I didn't know about this, would need to do research as to its implementation)
- Redundant Load/Store Elimination
- Scalar renaming

After this phase, which includes one or a few of the above (which is not an exhaustive list), it is in its cleanest state.

### Identifying Memory Adjacent References

From here I began to have problems with how I viewed the algorithm, aand it's quite simple, actually. This point begins the packing algorithm, forming PackSets. Basically, a PackSet is a set of packs, and a pack is an n-tuple where thee values are independent isomorphic statements in a basic block. The core of this step involves scanning each basic block to find pairs of independent statements, and checks for isomorphism, independence, position consistency and alignment consistency.
