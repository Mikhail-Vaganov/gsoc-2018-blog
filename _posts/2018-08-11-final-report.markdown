---
layout: post
title:  "GSoC 2018 - Final Report"
date:   2018-08-11 23:00:00 +0500
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/latest.js?config=TeX-MML-AM_CHTML">
</script>

[link_to_NBodySimulator]: https://github.com/JuliaDiffEq/NBodySimulator.jl

## What Was the Goal?
For the GSoC 2018 event I proposed to develop tools for handling N-body simulations. It was planned to develop an interface system of potentials, forces, constraints, thermostating, which will allow one to use those components as construction blocks during the implementation of arbitrary N-body simulations.

## [NbodySimulator][link_to_NBodySimulator]

Using the tools different systems of N-bodies can be simulated:
- charged particles
- magnetic particles
- neutral atoms and molecules
- celestial bodies
- water and liquid argon

Flexible developent of new systems is possible thanks to the following implemented potentials:
- elstrostatic
- magnetostatic
- Lennard-Jones
- harmonic bonds between particles
- harmonic angle mad of pairs of bonds

One even can define a custom potential using interface of NBodySimulator described in the documentation and create its own structure for a system of particles or just use universal `PotentialNBodySystem`.

Specifically for molecular dynamics simulations four tools for different thermostating methods were created:
- Anderesen thermostat
- Berendsen thermostat
- Nosé-Hoover thermostat
- Langevin thermostat

For analyzing results of simulation one can use the following tools:
- Radial distribution function calculation
- Mean squared displacement calculation
- Kinetic, potential, total energy functions

Radial distribution of liquid argon particles is shown in the next figure.
![rdf of liquid argon](https://user-images.githubusercontent.com/16945627/44006432-c86272e6-9e9d-11e8-92fd-3d539f07ed59.png)

Integration of NBodySimulator.jl with [Makie.jl](https://github.com/JuliaPlots/Makie.jl) via its recipes allows one to create animations of water molecules:
![animation of water molecules](http://makie.juliaplots.org/stable/media/type_recipe_for_molecule_simulation.mp4)

## Difficulties
The first trouble I faced with was implementation of periodic boundary conditions. It seems obvious for me now how to force particles going through one of the simulation cell boundary to appear at the opposite side of the box. But during the first weeks of codding for the GSoC, particles in my simulations were jumping all over the box, suddenly approaching to each other, creating strong repullsive force... and making the simulation finish with failure.

The second problem was the Langevin thermostating for water. It turned out to be difficult to understand how to write equations of the Langevin dynamics and therefore how to write code for molecules consisting of different atoms. Nevertherless, the problem was solved. Here is the example of thermostating the SPC/Fw model of water at 300 K:
![langevin thermostating for water](https://user-images.githubusercontent.com/16945627/44005987-c17556f4-9e95-11e8-9324-0d56ee3e74d3.png)

## Further work
Certainly the work under development and inprovement of the tools should be continued.
- Performance improvements are the top of the list, because to run calculations quickly is what the computer simulations were developed for.
- Implementation of the Ewald summation or other algorithms for approximation of the long range potentials is highly needed for correct calculations.
- The Nosé-Hoover chains or more advanced MD water models (TIP4P/2005, OPC, etc.) would be interesting for molecular dynamics simulations.
- Handling rotation of magnetic moments will make NBodySimulator a greate tool for simulating ferromagnetic liquids and elastomers.
- For preparing further instructions and advice for users it can be useful to compare different integrators of differential equation from [JuliaDiffEq](https://github.com/JuliaDiffEq) solving N-body problems.

## Documentation
The documentation for N-body simulation tools consists of 
- the README.md file in [NBodySimulator project][link_to_NBodySimulator]
- [scripts](https://github.com/JuliaDiffEq/NBodySimulator.jl/tree/master/examples) inside `examples` folder
- [posts](https://mikhail-vaganov.github.io/gsoc-2018-blog/) of this blog 

## Code
All the features and tools I developed during the GSoC event were separated into a brand new project called [NBodySimulator.jl][link_to_NBodySimulator].

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