---
layout: post
title:  "GSoC 2018 - the Final Report"
date:   2018-08-11 23:00:00 +0500
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/latest.js?config=TeX-MML-AM_CHTML">
</script>

## What Was the Goal?
For the GSoC 2018 event I proposed to develop tools for handling N-body simulations. It was planned to develop an interface system of potentials, forces, constraints, thermostating, which will allow one to use those components as construction blocks during the implementation of arbitrary N-body simulations.



## Documentation
The documentation for N-body simulation tools consists of 
- the README.md file in [NBodySimulator project](https://github.com/JuliaDiffEq/NBodySimulator.jl)
- [scripts](https://github.com/JuliaDiffEq/NBodySimulator.jl/tree/master/examples) inside `examples` folder
- [posts](https://mikhail-vaganov.github.io/gsoc-2018-blog/) of this blog 

## Code
All the features and tools I developed during the GSoC event were separated into a brand new project called [NBodySimulator.jl](https://github.com/JuliaDiffEq/NBodySimulator.jls).

But before that the developent was proceeding inside [DiffQePhysics.jl](https://github.com/JuliaDiffEq/DiffEqPhysics.jl) project.

This is the list of my pull request:

**DiffEqPhysics.jl**
- [#26](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/26)
- [#30](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/30)
- [#31](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/31)
- [#32](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/32)
- [#33](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/33)

**NBodySimulator**
- [#1](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/1)
- [#2](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/2)
- [#3](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/3)
- [#5](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/5)
- [#7](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/7)
- [#8](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/8)