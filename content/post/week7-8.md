---
title: "GSoC'21 : JuliaCon '21 & 8 Weeks done! "
date: 2021-08-02T18:40:33+05:30
draft: false
showReadingTime: true
---

<p align="center">
    <img src="https://user-images.githubusercontent.com/39168576/128030519-fba49477-596e-4755-ac32-48900d427c50.png" alt="drawing2" />
</p> 
<hr>

# **JuliaCon '21**
Ola Amigos! I'm super-excited to tell you guys that I attended my first [***JuliaCon***](https://juliacon.org/2021/) this year!! This is basically a Julia organized conference where people working in the Julia Community come together and give a briefing about the latest advancements, milestones and future prospects the organization looks up to. Some very promising works were shared. One of them, I recall, was from my mentor Chris Rackauckas about the **JuliaSIM** project. That's a next generation cloud based simulation platform, which is showing some promising results as it has marked about 700x enhancement for some of its clients compared to the State of the Art tech. The Language has also been the first choice (Vs Python, MATLAB etc.) for majority of the people who use it in one way or the other. Below are the references to some of these exciting talks I attended:
- ***<u>[https://youtu.be/lNbU5jNp67s](https://youtu.be/lNbU5jNp67s)</u> (_JuliaSIM_)***
- ***<u>[https://youtu.be/0XSk5zybfic](https://youtu.be/0XSk5zybfic)</u> (_Julia Developer Survey Results 2021_)***

I wish perhaps we can be a part of this some day :D 

## **Loop Vectorization**

So let's discuss some work related progress now.
Lately, I've been dedicated in optimizing the `VCOPs` code. We've discussed how we can make our code Cache-efficient by addressing cache-misses i.e. making the processing units point to a contiguous set of memory so that switches among such chunks are minimized. But specifically speaking in reference to Scientific Computing, a lot of packages are developed so that a run gets performed in as minimal time as possible. One such package developed for the `SciML` organization is [`_LoopVectorization.jl_`](https://github.com/JuliaSIMD/LoopVectorization.jl). It does exactly what the name suggests : **vectorizing loops**.

But what does that fancy term mean?? In nut-shell, in **parallel systems**, if we have nested loop operations such that the order of compution is irrelevant then we can distribute a single operation across multiple processors for varying operands. This doesn't end here, we want to avoid conditionals in inner loops as much as possible. They will cause branch checks, which can stop [***SIMD(Single Instruction Multiple Data)***](http://kristofferc.github.io/post/intrinsics/) from being possible. If we can gauruntee these conditions in our run, then LoopVectorization is the way to go for fast processing!

`LoopVectorization.jl` provides exactly that. This library provides the `@turbo` macro, which may be used to prefix a `for` loop or `broadcast` statement. It then tries to vectorize the loop to improve runtime performance.

Mentioned below are some benchmark improvements I got by putting that to use in the internal `CenteredDifference` convolutions of our Operators, for a 101x101x101 3D function/Vector :

- ***Earlier***
```
Divergence
@btime u = A*u0
  31.761 μs (15 allocations: 3.52 KiB)

Gradient
@btime u = A*u0
  30.562 μs (16 allocations: 8.95 KiB)

Curl
@btime u = A*u0
  42.645 μs (15 allocations: 8.94 KiB)
```

- ***Now***
```
Divergence
@btime u = A*u0
  7.945 μs (15 allocations: 3.52 KiB)

Gradient
@btime u = A*u0
  8.528 μs (16 allocations: 8.95 KiB)

Curl
@btime u = A*u0
  10.876 μs (15 allocations: 8.94 KiB)
```
That's around **4x speed-up** which is massive for large resource-intensive runs!!
I think we have came a long way, beginning with a runtime of **~140 μs** and finally touching at **~ 7μs**. Perhaps, that would be good enough for the time being :D

- ***Link to the PR : <u>[https://github.com/SciML/DiffEqOperators.jl/pull/428](https://github.com/SciML/DiffEqOperators.jl/pull/428)</u>***
- ***Link to the PR : <u>[https://github.com/SciML/DiffEqOperators.jl/pull/431](https://github.com/SciML/DiffEqOperators.jl/pull/431)</u>***

Next, I'm looking to dive in and enhance some [***Spare Array / BandedMatrix***](https://juliamatrices.github.io/BandedMatrices.jl/latest/) representations of our Linear Derivative Operators.
Stay safe, until next time **:)**  

<hr>
<p align="center">
    <img src="https://user-images.githubusercontent.com/39168576/119239386-4e523200-bb66-11eb-8a36-46fcf42c92a8.png" alt="drawing" />
</p> 
