---
layout: post
title:  "Potentials for the SPC/Fw water simulation"
date:   2018-07-09 23:00:00 +0500
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/latest.js?config=TeX-MML-AM_CHTML">
</script>

For development of tools for molecular dynamics simulations we needed to implement one of the well-known models of molecules. For that purpose I chose the SPC/Fw model of water. 

The molecule structure of $$H_2O$$ is preserved under action of two components of intramolecular potentials.
The first one is a harmonic bond potential controlling distances between atoms of hydrogen and the central atom of oxygen. 
The second one is also a harmonic  potential preserving the desired valence angle between those atomic bonds.

$$ V^{intra}=\frac{k_b}{2}\Big[(r_{OH_1}-r^0_{OH})^2+(r_{OH_2}-r^0_{OH})^2\Big] + \frac{k_a}{2}(\theta_{\angle HOH}-\theta_{\angle HOH}^0)^2$$

where $$r^0_{OH}$$ and $$\theta_{\angle HOH}^0$$ are the equilibrium bond length and angle, $$k_a$$ and $$k_b$$ are coefficients of the bond and valence angle elasticity.

Molecules of water interact by means of the Lennard-Jones and Coulomb intermolecular potentials:

$$ V^{inter}=\sum_{i,j}^{all pairs}\Big\{4\epsilon_{ij}\Big[\Big(\frac{\sigma_{ij}}{r_{ij}}\Big)^{12}-\Big(\frac{\sigma_{ij}}{r_{ij}}\Big)^{6}\Big]+\frac{q_iq_j}{r_{ij}}\Big\}$$

The implemented functions for the mentioned potentials and corresponding accelerations can be used in simulations of arbitrary molecules.

During simulation of water the law of energy conservation is satisfied:

<img src="https://user-images.githubusercontent.com/16945627/42471765-5967689a-83d8-11e8-9cf5-790e8ab33947.png">

### Radial distribution function
Radial distribution function ([RDF](https://en.wikipedia.org/wiki/Radial_distribution_function)) is a very important tool in molecular dynamics.

{% highlight julia %}
(rs, grf) = rdf(result)
{% endhighlight %}

Here I present results of simulations of the two well-known systems.
First of all, the RDF for liquid argon:
![rdf for liquid argon 343 particles 10000 steps](https://user-images.githubusercontent.com/16945627/41996141-6867fe5a-7a6d-11e8-92a9-24e9b99b7ebd.png)

And the RDF for the water SPC/Fw model (216 particles, from 0 till 1.5 ps by 0.5 fs time step):
![rdf for water 216 particles 2997 steps](https://user-images.githubusercontent.com/16945627/42346082-d1177e5a-80ba-11e8-9fc4-141a61b3ab3f.png)

Mean-square displacement may be used for estimation of particles diffusion.

{% highlight julia %}
(ts, dr2) = msd(result)
{% endhighlight %}

![msd for water 216 particles 2997 steps](https://user-images.githubusercontent.com/16945627/41996180-8b9667ae-7a6d-11e8-9aa6-b0a441064e82.png)

### Visualization in VMD
In order to export results of simulations, namely trajectories, I have implemented saving positions of particles at every timestep in a PDB file.

{% highlight julia %}
save_to_pdb(result, path_to_the_file )
{% endhighlight %}

<img src="https://user-images.githubusercontent.com/16945627/42470151-0774a85e-83d3-11e8-9ca6-6a5925848d62.gif" alt="Here should have appeared a gif with moving water molecules">

### Summary
All tools, that I develop for n-body simulations, are located in [DiffEqPhysics.jl](https://github.com/JuliaDiffEq/DiffEqPhysics.jl) project. The `test` and `examples` folder contains examples of all physical interactions already implemented. At the moment of writing this post, the following items can be used in simulations:
- gravitational
- electrostatic
- magnetostatic
- based on the [Lennard-Jones potential](https://en.wikipedia.org/wiki/Lennard-Jones_potential)
- custom pair-wise interaction implemented via the `CustomAccelerationSystem` interface
- harmonic bonds between particles
- harmonic potential for valence angle between atoms
- radial distribution function
- mean swuare displacement


