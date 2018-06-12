---
layout: post
title:  "Starting point"
date:   2018-05-28 01:01:01 +0500
---

### Before GSoC 2018
I heard a lot of exciting stories from my colleagues  and mates about their experience at GSoCs. Some of them proudly showed me the features they implemented in open source projects, some of them told what cool technology they learned during summer. Anyway, they ignited my interest in participating in the GSoC.
Nevertheless for several years it was almost impossible, since I already had a full time job. The situation changed when I decided to attend the postgraduate courses and began my scientific career. This spring I was again a student having opportunity to contribute to one of many amazing organizations of the GSoC 2018 event.

### Choosing the project
{% highlight julia %}
julia> println("Hello world and GSoC 2018")
{% endhighlight %}
I definitely wanted to find a community and propose a project which would be connected with my study or at least with physics. Though CERN's ideas sounded gripping (and sometimes frightening), they were almost completely about mathematics or machine learning. Luckily, I found another community of relatively new programming language, called Julia, that had among other ideas one of a particular interest for me. It was _"Tooling for molecular dynamics and N-body simulations"_. From that I started preparing my proposal and learning Julia, certainly.

### The project
I proposed to develop tools for handling N-body simulations in [DiffEqPhysics.jl](https://github.com/JuliaDiffEq/DiffEqPhysics.jl) project. 

During the GSoC 2018, an interface for creating N-body simulations will be designed and a set of well-known specific problems will be implemented based on that interface. Users will be able to access the predefined models for simulating specific problems, but at the same time, they will be able to construct their own representation of an N-body problem for their particular task using our generic interfaces. It is planned to develop an interface system of potentials, forces, constraints, thermostating, which will allow one to use those components as construction blocks during the implementation of arbitrary N-body simulations.

### Coding started
During the first two weeks, I developed the first version of the interface making it easier to create particles and systems of those particles for N-body simulations. Under the hood of the interface, the transformation of N-body systems and simulation conditions into problems in terms of differential equations takes place.
I also implemented the gravitational and electrostatic interactions of particles and provided my code with necessary tests. Here is my first [PR](https://github.com/JuliaDiffEq/DiffEqPhysics.jl/pull/31).



