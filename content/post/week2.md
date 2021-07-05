---
title: "GSoC'21 : Approaching the First Evaluations"
date: 2021-07-04T23:29:33+05:30
draft: false
showReadingTime: true
---

## **Vector Calculus Operators**

Hey guys! It has been some time since I last updated you since the community bonding period. Well, I've been pretty busy diving in some exciting stuff :P. 

***Note : Here, Vector has been referred in the physical sense rather than the Vector Type in `julia`.*** 

I started the coding period picking up my work with the vcops (i.e. Vector Calculus Operators).
The idea has been to build support for basic vector operations on a given discretized function/vector for the DiffEqOperators package, SciML. In discretized form on a grid space, a N-variable function would basically take the form of a N-dimensional Matrix with each entry being its value at that grid point. Similar to that, a N-component Vector `A` would be the same except that each entry would rather be a 1-D array where an entry indexed `i` would represent the component `Aᵢ`of `A`, similar to Tensor. 
Keeping this in mind, the task was to make lazy operators and commonly used functions.

Here are the functionalities that have implemented :
- `Gradient` Operator
- `Curl` Operator
- `Divergence` Operator
- `Dot` & `Cross` of 2 vectors (extended from the `LinearAlgebra.jl` library)
- `Norm` of given vector (extended from the `LinearAlgebra.jl` library)

The Operators are essentially structures that store the `CenteredDifference` operator sequentially upto the Nth-axis for performing Finite-difference operations on the input. These operators differ in the way convolutions are carried by them with Gradient and Curl accepting vector inputs while Divergence sufficing with functions. The convolutions would be carried out through deriving the partial derivatives `∂f/∂xₙ` along various dimensions by permuting with the stored CD-structures. `Norm` , `Dot` &  `Cross` provided in the LinearAlgebra.jl package worked for 1-D input. Hence, I constructed wrappers extending their support to multi-dimensions through broadcasting.

# Blocks and Workarounds
These functionalities have been tested for support with AbstractArray inputs, but an inclusion for `BoundaryPaddedArray` was required. For that, I first needed to extend the `MultiDimensionBC` operator for our Tensor. But after digging in, I found that the current implementation had been goofed-up since it only provided support for padding only with constant Boundary Value across the whole grid-space of one axis. For user convenience, rather than asking for BCs at all extremes along an axis, I have implemented an extrapolation scheme using CenteredDiff. and developed the `MultiDimBC` construct storing the CenteredDiff. along various axis for computing partial derivatives through which we would extrapolate.
This still needs enhancement since this isn't exactly an affine operator
The PR for the implementation can be found at : [https://github.com/SciML/DiffEqOperators.jl/pull/414](https://github.com/SciML/DiffEqOperators.jl/pull/414)

# Way forward
The first Goal has almost been achieved, I'm currently optimizing the code for memory issues and hope to get this done before the First Evaluations (which are fast approaching!!). 
You can find this work at : [https://github.com/SciML/DiffEqOperators.jl/pull/375](https://github.com/SciML/DiffEqOperators.jl/pull/375)
Pinge me if you wish to know more, until next time!

<hr>
<p align="center">
    <img src="https://user-images.githubusercontent.com/39168576/119239386-4e523200-bb66-11eb-8a36-46fcf42c92a8.png" alt="drawing" />
</p> 