---
layout: post
title:  "GSoC 2018 - Final Report"
date:   2018-08-11 23:00:00 +0500
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/latest.js?config=TeX-MML-AM_CHTML">
</script>

[link_to_NBodySimulator]: https://github.com/JuliaDiffEq/NBodySimulator.jl
[link_to_DiffEqPhysics]: https://github.com/JuliaDiffEq/DiffEqPhysics.jl
[link_to_JuliaDiffEq]: https://github.com/JuliaDiffEq

## What Was the Goal?
For the GSoC 2018 event I proposed to develop tools for handling N-body simulations. It was planned to develop an interface system of potentials, forces, constraints, thermostating, which will allow one to use those components as construction blocks during the implementation of arbitrary N-body simulations.

## NBodySimulator.jl
During the GSoC 2018 such tools were created in [DiffEqPhysics][link_to_DiffEqPhysics] project, devoted to solving physical problems formulated in terms of differential equations. At some point of coding period there were enough tools and features for N-body simulations to separate them into their own project.

[**NBodySimulator.jl**][link_to_NBodySimulator] was created in [JuliaDiffEq][link_to_JuliaDiffEq] organization as a container of tools for N-body simulations, including molecular dynamics, interaction of celestial bodies, charged particles, etc. Using the tools different systems of N-bodies can be modelled:
- charged particles
- magnetic particles
- neutral atoms and molecules
- celestial bodies
- SPC/Fw water model
- liquid argon and other Lennard-Jones fluids

In the field of gravitationally interacting bodies, choreography is a periodic solution in which all the bodies are equally spread out along a single orbit. Here is an example of such 5-body choreography modelled using NBodySimulator:

<img src="https://user-images.githubusercontent.com/16945627/44007423-8e36059a-9eae-11e8-8b4e-e76459ccc1a0.gif" alt="Here should appear a gif of the 5-body choreography" width="380"/>

Flexible developent of new systems is possible thanks to the following implemented potentials:
- electrostatic
- magnetostatic
- Lennard-Jones
- harmonic bonds between particles
- harmonic angle made from pairs of bonds

One even can define a custom potential using interface of NBodySimulator described in the [documentation](https://github.com/JuliaDiffEq/NBodySimulator.jl/blob/master/README.md) and create their own structure for a system of particles or just use universal `PotentialNBodySystem`.

Specifically for molecular dynamics simulations four thermostats were created:
- Anderesen thermostat
- Berendsen thermostat
- Nosé-Hoover thermostat
- Langevin thermostat

For analyzing results of simulations one can use the following tools:
- Radial distribution function calculation
- Mean squared displacement calculation
- Kinetic, potential, total energy functions

Radial distribution of liquid argon particles is shown in the next figure.
![rdf of liquid argon](https://user-images.githubusercontent.com/16945627/44006432-c86272e6-9e9d-11e8-92fd-3d539f07ed59.png)

Integration of NBodySimulator.jl with [Makie.jl](https://github.com/JuliaPlots/Makie.jl) via its [recipes](http://makie.juliaplots.org/stable/examples-meshscatter.html#Type-recipe-for-molecule-simulation-1) allows one to create animations of water molecules:
<video controls="" autoplay="" loop="" muted="">
      <source src="http://makie.juliaplots.org/stable/media\type_recipe_for_molecule_simulation.mp4" type="video/mp4">
      Your browser does not support mp4. Please use a modern browser like Chrome or Firefox.
</video>

It is also possible to import data from and export data to [PDB](https://en.wikipedia.org/wiki/Protein_Data_Bank_(file_format)) files, and then, for example, visualize particles in [VMD](http://www.ks.uiuc.edu/Research/vmd/) software. Though, it is fully implemented only for water for now.

## Difficulties
The first trouble I faced with was implementation of periodic boundary conditions. It seems obvious for me now how to force particles going through one of the simulation cell boundary to appear at the opposite side of the box. But during the first weeks of coding for the GSoC, particles in my simulations were jumping all over the box, suddenly approaching to each other, creating strong repullsive force... and making the simulation finish with failure.

The second problem was the Langevin thermostating for water. It turned out to be difficult to understand how to write equations of the Langevin dynamics and therefore how to write code for molecules consisting of different atoms. Nevertherless, the problem was solved (the damping term of the Langevin equation should depend on the mass of an atom). Here is the example of thermostating the SPC/Fw model of water at 300 K:
![langevin thermostating for water](https://user-images.githubusercontent.com/16945627/44005987-c17556f4-9e95-11e8-9324-0d56ee3e74d3.png)

## Further Work
As the next important step I plan to submit an article about NBodySimulator.jl to [JOSS](http://joss.theoj.org/about#about).

Certainly the work under development and inprovement of the tools should be continued.
- Performance improvements are the top of the list, because duration of calculations is important and the package aims to be performance competitive with established projects in the field.
- Implementation of the Ewald summation or other algorithms for approximation of the long range potentials is highly needed for correct calculations.
- The Nosé-Hoover chains or more advanced MD water models (TIP4P/2005, OPC, etc.) would be interesting for molecular dynamics simulations.
- Handling rotation of magnetic moments will make NBodySimulator a great tool for simulating ferromagnetic fluids and elastomers.
- For preparing further instructions and advice for users it can be useful to compare different integrators of differential equation from [JuliaDiffEq][link_to_JuliaDiffEq] solving N-body problems.

## Documentation
The documentation for N-body simulation tools consists of 
- the README.md file in [NBodySimulator project][link_to_NBodySimulator]
- [scripts](https://github.com/JuliaDiffEq/NBodySimulator.jl/tree/master/examples) inside `examples` folder
- [posts](https://mikhail-vaganov.github.io/gsoc-2018-blog/) of this blog 

## Code
All the features and tools I developed during the GSoC event were separated into a brand new project called [NBodySimulator.jl][link_to_NBodySimulator].

But before that the development was proceeding inside [DiffEqPhysics.jl][link_to_DiffEqPhysics] project.

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

## Conclusion
All the tasks and goals of the GSoC project were completed.

Further work will be devoted to submitting a JOSS article and improvement of **NBodySimulator** project in order to make it a full-fledged open source tool for N-body simulations.