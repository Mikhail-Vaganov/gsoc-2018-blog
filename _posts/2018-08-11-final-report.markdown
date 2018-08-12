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

## NbodySimulator


## Difficulties
The first trouble I faced with was implementation of periodic boundary conditions. It seems obvious for me now how to force particles going through one of the simulation cell boundary to appear at the opposite side of the box. But during the first weeks of codding for the GSoC, particles in my simulations were jumping all over the box, suddenly approaching to each other, creating strong repullsive force... and making the simulation finish with failure.

The second problem was the Langevin thermostating for water. It turned out to be difficult to understand how to write equations of the Langevin dynamics and therefore how to write code for molecules consisting of different atoms. Nevertherless, the problem was solved. Here is the example of thermostating the SPC/Fw model of water at 300 K:
![langevin thermostating for water](https://user-images.githubusercontent.com/16945627/44005987-c17556f4-9e95-11e8-9324-0d56ee3e74d3.png)

## Further work
Certainly the work under development and inprovement of the tools should be continued.
- Performance improvements are the top of ist, because to run calculations quickly is what the computer simulations were developed for.
- Implementation of the Ewald summation or other algorithms for approximation of the long range potentials is highly needed for correct calculations.
- The Nosé-Hoover chains or more advanced MD water models (TIP4P/2005, OPC, etc.) would be interesting for molecular dynamics simulations.
- Handling rotation of magnetic moments will make NBodySimulator a greate tool for simulating ferromagnetic liquids and elastomers.
- For preparing further instructions and advice for users it can be useful to compare different integrators of differential equation from [JuliaDiffEq](https://github.com/JuliaDiffEq) solving N-body problems.

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
- [#26 "Convenient interface types for the Nbody gravitational problem"](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/26), 
- [#30 "Added implementation for the gravitational n-body simmulations"](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/30)
- [#31 "Nbody structs"](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/31)
- [#32 "Improving tools while conducting water simulations"](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/32)
- [#33 "Different thermostat algorithms"](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/33)

**NBodySimulator**
- [#1 "changed configuration for appveyor"](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/1)
- [#2 "N-body simulations are moving here from DiffEqPhysics"](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/2)
- [#3 "Attempt to repair the testing"](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/3)
- [#5 "Improved testing"](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/5)
- [#7 "Documentation"](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/7)
- [#8 "Renaming file NBodySimulator"](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/8)