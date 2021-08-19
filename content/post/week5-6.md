---
title: "GSoC'21 : Moving Ahead"
date: 2021-07-19T23:29:33+05:30
draft: false
showReadingTime: true
---

## **Cache efficiency**

Hello Everyone! Some good news, the first evaluations happened from 12th-16th of July and I aced them!

Last time I demonstrated my first sub-goal `(Vector Calculus Operators)` and how my next task was to optimize the code. 
So, after assessment we found that there were 2 major inefficiencies present -
1. Unnecessary Allocations
2. Cache Line Inefficiencies

Lets talk about both of these in order and what ways I've adopted to address them!

# _Reducing the Allocations_

Remember how we opted to represent a `N`- dim physical Vector `A` in Tensor form? I was using a `1-D array` to store its various components at a grid point. The problem with that is as soon as we use an array for storage, this will require allocations! For e.g. , a 3-component Vector would be initialized as `zeros(T,3)` (`T` being the type of element) and this will result in allocations. hence if we have a grid space  of size `p x q x r`, there will be that many extra allocations of similar order. That's a really bad idea to use so much memory in chunks since our CPU cache will use the heap that many times and allocate at places which will not necessarily be contiguous. The end result is that the reference pointer has to make switches whenever it tries to access a different chunk of memory on the heap.
This really slows down our computations.

Hence, I decided to dispose off that representation and insead add an extra dimension in our Tensor, whose indices will be holding the respective components our Vector. This way we will skip the array allocations and whatever allocations we will have would be more or less contiguous. I tried this and I was able to bring down the no. of allocations as well as speed up the operations. Here is a summary of performance prior to & after the changes - 

**Prior Implementation :**
```
  memory estimate:  746.22 KiB
  allocs estimate:  14804
  --------------
  minimum time:     468.595 μs (0.00% GC)
  median time:      544.707 μs (0.00% GC)
  mean time:        616.459 μs (11.55% GC)
  maximum time:     8.308 ms (92.88% GC)
  --------------
  samples:          8063
  evals/sample:     1
```

**Post the structural changes :**
```40.521 μs (15 allocations: 8.94 KiB)```

This is a **10x** speedup in performance and around **100** times lesser memory usage! So yes, the take away is to ensure you use contiguous blocks of memory wherever you can :)

# _Cache Line Inefficiencies_

To minimize cache misses, modern CPUs try to guess the next chunk of memory we will try to access while traversing arrays. Hence, it makes a guess and grabs a block known as **cache line** that it supposes we will access next. The way of guessing this varies across different languages. Since `SciML` is a `julia` intensive package, the convention in `julia` is that the linear array of memory is formed by stacking the columns one after another i.e. it is **Column-Major**. One has to keep this thing in mind while iterating.

<p align="center">
    <img src="https://user-images.githubusercontent.com/39168576/126272152-f8397608-e41e-4332-9ca0-fb4ca7b2ef4b.png" alt="diagram" />
</p> 

***Source : [https://mitmath.github.io/18337/lecture2/optimizing](https://mitmath.github.io/18337/lecture2/optimizing)***

Earlier my implementations were **Row-Major** i.e. I had written the loops of our convolutions in a way that iterated along rows (in a matrix). This will again bring up the same issue discussed in the previous section, of reference pointer requiring many switches during traversal. Therefore, once this thing was pointed out by my mentor, I changed that at most of the places and voila! Some more performance enhancements achieved.

- ***Link to the PR : <u>[https://github.com/SciML/DiffEqOperators.jl/pull/427](https://github.com/SciML/DiffEqOperators.jl/pull/427)</u>***

The next thing in sight would be to start-off with my second subgoal - **Diversifying functionalities for `MOLFiniteDifference`**, the symbolic handling tool of our library. 
Hope you liked this read, some more coming up soon! 

<hr>
<p align="center">
    <img src="https://user-images.githubusercontent.com/39168576/119239386-4e523200-bb66-11eb-8a36-46fcf42c92a8.png" alt="drawing" />
</p> 
