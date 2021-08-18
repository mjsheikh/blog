---
title: "GSoC'21 : Last Mile"
date: 2021-08-16T23:30:16+05:30
draft: false
showReadingTime: true
---

# **Closing in Coding Period**
Hello to Everyone! Into the final couple weeks of my journey at GSoC'21, I've tried accomplishing the remaining couple of proposed goals of mine. The first being concerned with generating efficient concretizations of [**sparse matrices**](https://en.wikipedia.org/wiki/Sparse_matrix) while the later aiming to enhance the [**ModelingToolKit.jl**](https://github.com/SciML/ModelingToolkit.jl) extension in our library. Let's walk through each one of them.

#### _Concretizing Operators_

**Concretization** here refers to casting the lazy, non-allocating operators into normal matrix types. This helps users to visualize the underlying sparse matrices which are being used either as **linear** or **affine** operators. Since the operation is allocating, we would like to ensure it gets carried out while putting minimum resources to use.

Broadly, there are 4 ways to concretize a lazy operator : 
- `BandedMatrix` : Casting stencils to normal matrices, allocating memory only for stencil entries in a row.  
- `Array` : Initializing a fully allocated array with zeros filling in the non-stenicil regions. This is rather the most resource intensive implementation.
- `SparseMatrix` : Only allocating for non-zero entries of stencils. This is the least allocating of all casts.
- `BlockBandedMatrix`: Similar to BandedMatrix, used for operations in multi-dimensional spaces.  

Given this, in terms of optimization I reduced **bandwidths** in the `BandedMatrix` form of concretization for `CenteredDifference` operator since they were unnecessarily large earlier, probably because of over-estimating the maximum size it should take. 
Here's an example demonstrating the change for `CenteredDifference(3,3,0.1,10)` (notice the reduction in bandwidth):

<p align="center">
    <img src="https://user-images.githubusercontent.com/39168576/129663434-cdab7473-dea3-472c-bc46-9bc749fde1b4.gif" alt="gif" />
</p> 

`UpwindDifference` was updated some while ago with an `offside` feature that allows users to have some offiside points against the direction of  primary wind. In full capacity, this can allow it to behave as CenteredDifference. I have synced up the all concretization forms for the operator with this modality.

Most BC operators in the package aren't linear but rather affine i.e. they operate in the form `Q_l*u + Q_b`. I've worked extensively, building all forms of casting support for these in the form of a 2-element array storing`Q_l` and `Q_b` respectively, both of which bear **minimum required allocations**.

- ***Link to the PR : <u>[https://github.com/SciML/DiffEqOperators.jl/pull/432](https://github.com/SciML/DiffEqOperators.jl/pull/432)</u>***

#### _Enhancing MOLFiniteDifference_

[**ModelingToolKit.jl**](https://github.com/SciML/ModelingToolkit.jl) provides symbolic interface for handling equations. `MOLFiniteDifference` is its extension for [**DiffEqOperators**](https://github.com/SciML/DiffEqOperators.jl) package, symbolically handling pde systems. I generalized its handling of BC conditions to support N-order derivatives by storing particular FiniteDifference stencils required for handling the derivates present in the system, skipping whichever weren't there & hence avoiding any unnecessary storage.

- ***Link to the PR : <u>[_https://github.com/SciML/DiffEqOperators.jl/pull/435_](https://github.com/SciML/DiffEqOperators.jl/pull/435)</u>***
<hr>
<p align="center">
    <img src="https://user-images.githubusercontent.com/39168576/119239386-4e523200-bb66-11eb-8a36-46fcf42c92a8.png" alt="drawing" />
</p> 

