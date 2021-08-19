---
title: "GSoC'21 : Final Report"
date: 2021-08-19T12:43:16+05:30
draft: false
showReadingTime: true
---

<p align="center">
    <img src="https://user-images.githubusercontent.com/39168576/119238813-8d7e8400-bb62-11eb-881a-f5ec35cb4b19.png" alt="NumFOCUS_LRG" />
</p>

**Name** : Mohd Jeeshan Sheikh <br>
**Organization** : NumFOCUS <br>
**Mentors** : Chris Rackauckas, Kanav gupta, Yingbo Ma <br>
**Project** : ***<u>[Discretizations of Partial Differential Equations](https://drive.google.com/file/d/1-eGbu8bG8GkSOYYZfApMCppYqQvuPteF/view)</u>***

Greetings to everyone! Finally my journey through various walks of GSoC'21, starting from preps to application, acceptance and ultimately gettings things done in sync with my proposal has came to end. Big thanks to my mentors, especially <u>[**Chris Rackauckas**](https://chrisrackauckas.com/)</u> for deciding to commit with me on this project, engaging in insightful discussions and providing his inputs at several pivotal instances.In retrospect, I came in with little idea about the SciML org and will be walking out with a really comprehensive knowledge about <u>[**DiffEqOperators.jl**](https://github.com/SciML/DiffEqOperators.jl)</u>, one of the many awesome and state-of-the-art packages that the organization has developed over the years which prove to be miles ahead in terms of performance in comparison to many other commercially popular tools in the domain of scientific computing.s

#### **Summary of Things Done**
My work can be summarized to lie under two categories : <br>
- **Feature enhancements** <br>
- **Optimizations** <br>

I took the task to look for potentially desirable but missing components in DiffEqOperators.jl and introduced Vector Calculus Operators, which carry wide applications in scientific domain. This sub-goal included designing the structures, methods and various operations for the operators and finally working towards their optimization to match the API standards and benchmarks. During this I learnt the concepts of cache-efficiency and vectorization of loops. Having acquired this knowledge, I also benchmarked the convolutions for DerivativeOperators and generated a speed-up of over **2x** for uniform grid CenteredDifference convolutions (though there were observed improvements in others as well!). Moving on, I designed multiple concretizations for AffineBC operators, synced concretizations for updated UpwindDifference and reduced the bandwidths in BandedMatrices for Derivative (and as a result GhostDerivative) Operators. In the end, the symbolic handling by MOLFiniteDifference was extended to support a general N-ordered BC in 1D space.

A more detailed account of these works can be found in the following blog posts :
- <u>[<b>GSoC'21 : Last Mile</b>](https://mjsheikh.github.io/blog/post/week9-10/)</u>
- <u>[<b>GSoC'21 : Julia Con'21 and 8 Weeks done!</b>](https://mjsheikh.github.io/blog/post/week7-8/)</u>
- <u>[<b>GSoC'21 : Moving forward</b>](https://mjsheikh.github.io/blog/post/week5-6/)</u>
- <u>[<b>GSoC'21 : Approaching the First Evaluations</b>](https://mjsheikh.github.io/blog/post/week2/)</u>
- <u>[<b>Commencing GSoC'21 with NumFOCUS</b>](https://mjsheikh.github.io/blog/post/commencement/)</u>

#### <u>**Compilation of all contributions**</u>
<table class="u-full-width">
  <thead>
    <tr>
      <th>Description</th>
      <th>Pull Request</th>
      <th>Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Introduce NonLinearDiffusion function</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/327"><b>#327</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Docs update for NonLinearDiffusion</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/332"><b>#332</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Dimensional corrections for PeriodicBC</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/334"><b>#334</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Address type mismatch in GeneralBC</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/337"><b>#337</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Debug broken tests</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/340"><b>#340</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Operator Tutorials page</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/346"><b>#346</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Introduce Biased Upwind Operator</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/347"><b>#347</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Docs update for Biased Upwind</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/368"><b>#368</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Operators for Vector Calculus</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/375"><b>#375</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Multi-Dimensional BC Operator</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/414"><b>#414</b></a></u></td>
      <td>Open</td>
    </tr>
    <tr>
      <td>Reduce allocations and fix loop orders in VCOps</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/427"><b>#427</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>LoopVectorization in VCOps convolutions</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/428"><b>#428</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Vectorize loops in derivative operators convolutions</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/431"><b>#431</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>Concretizations : BC operator enhancements, bandwidth optimization <br>& restructuring for Derivative Operators</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/432"><b>#432</b></a></u></td>
      <td>Merged</td>
    </tr>
    <tr>
      <td>MOLFinitieDifference : support for N-ordered derivatives in BCs</td>
      <td><u><a href="https://github.com/SciML/DiffEqOperators.jl/pull/435"><b>#435</b></a></u></td>
      <td>Merged</td>
    </tr>
  </tbody>
</table>

#### **Future Prospects**
I feel privileged for landing the opportunity of working with this amazing global community, learning from the basic know-hows to some really advanced level stuff going around in development. I'll always carry this share of experience and try to incorporate it whenever and wherever possible.<br>
During my journey I was able to get past many blocks that came along the way and the purpose seems fulfilled. But then every ending is followed by a new beginning :) There are lots of sought out features like the symbolic interface provided by <u>[**ModelingToolkit.jl**](https://github.com/SciML/ModelingToolkit.jl)</u> that are be looked up to and would hopefully provide a more versatile experience to users.<br>Having said that, I would love to pitch in and collaborate with other contributors from time to time, for working towards the bigger goal of providing efficient and top-notch utilities for the scientific community by <u>[**SciML**](https://sciml.ai/)</u> and <u>[**The Julia Lang**](https://julialang.org/)</u>.

<hr>
<p align="center">
    <img src="https://user-images.githubusercontent.com/39168576/119239386-4e523200-bb66-11eb-8a36-46fcf42c92a8.png" alt="drawing" />
</p> 