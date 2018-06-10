---
layout: post
title:  "From dancing stars to liquid argon"
date:   2018-06-11 04:17:00 +0500
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/latest.js?config=TeX-MML-AM_CHTML">
</script>

During my GSoC-2018 [project](https://summerofcode.withgoogle.com/projects/#5697153159135232), I will develop tools for n-body simulations in Julia. Many physical systems can be described in terms of interacting particles or bodies. The result of simulating such systems is trajectories of particles, whose movements obey Newton's 2nd low:

$$ \frac{d\textbf{r}_i}{dt} = \textbf{v}_i $$

$$ m_i\frac{d\textbf{v}_i}{dt} = -\nabla{\Phi_i} $$

where $$\nabla{\Phi_i}$$ is the gradient of potential $$\Phi_i$$ at the place $$\textbf{r}_i$$ where the i-th particle is located. 

In case of such potentials as gravitational, electrostatic, magnetostatic, etc., we can directly write the corresponding force acting on the i-th particle:
 
$$ m_i\frac{d\textbf{v}_i}{dt} = \textbf{F}_i $$

In general, potentials and forces depend on particle properties $$\{x_j\}^N$$, their locations $$\textbf{r}$$, velocities $$\textbf{v}$$ and on time $$t$$:

$$ \Phi_i = \Phi_i(\{x_j\}^N,\textbf{r},\textbf{v},t) \kern 1pc \textbf{F}_i = \textbf{F}_i(\{x_j\}^N,\textbf{r},\textbf{v},t) $$

During first weeks of the GSoC I have implemented the calculation of several standard physical forces and the first version of interface for simulating n-body interactions. It is better to show usage of the interface with actual examples of simulations.

### N-body Choreography
In the field of gravitationally interacting bodies, choreography is a periodic solution in which all the bodies are equally spread out along a single orbit.

The famous figure-8 choreography can be simulated by following steps.

Firstly, we define bodies of our system:

{% highlight julia %}
m1 = MassBody(SVector(-0.995492, 0.0, 0.0), SVector(-0.347902, -0.53393, 0.0), 1.0)
m2 = MassBody(SVector(0.995492, 0.0, 0.0), SVector(-0.347902, -0.53393, 0.0), 1.0)
m3 = MassBody(SVector(0.0, 0.0, 0.0), SVector(0.695804, 1.067860, 0.0), 1.0)
{% endhighlight %}

Here, the first `SVector` defines initial position, and the second `SVector` corresponds to the velocity of a body. The third argument in the constructor for `MassBody` represents the mass of an initialized entity.

Next, we need to define the gravitational constant for correct calculations in a chosen measuring system:
{% highlight julia %}
G = 1
system = GravitationalSystem([m1, m2, m3], G)
{% endhighlight %}
Now we have the system, which we want to test during simulation. For conducting the simulation, we need to define conditions of our experiment: in this simple case of gravitationally interacting bodies, the only parameter of our experiment is its duration:
{% highlight julia %}
tspan = (0.0, 2pi);
simulation = NBodySimulation(system, tspan)
{% endhighlight %}
That is all: we have just set up our simulation. The following command will start the actual calculations:
{% highlight julia %}
sim_result = run_simulation(simulation)
{% endhighlight %}

The result of this simulation `sim_result` has two fields: the initial setting of `simulation` and `solution` of corresponding differential equations. The latter is discribed in the [documentation](http://docs.juliadiffeq.org/latest/basics/solution.html) of DifferentialEquations.jl.  

The animated 3-body choreography is presented in the following gif:
<img src="https://user-images.githubusercontent.com/16945627/41206054-76059a08-6d17-11e8-85be-ce1767188570.gif" alt="Here should appear a gif of moving bodies" width="350"/>

### Liquid argon
Molecular dynamics is another type of n-body simulations, utilized for studying the physical movements of atoms and molecules.
In the next simple example, it is shown how to simulate interactions of liquid argon atoms.
The Lennard-Jones potential defines behavior of such particles:

$$ \Phi_{LJ} = 4\epsilon \left [\left(\frac{\sigma}{r}\right)^{12}-\left(\frac{\sigma}{r}\right)^6 \right ] $$

These are usual physical parameters for liquid argon:
{% highlight julia %}
const T = 120.0 # °K
const kb = 1.38e-23 # J/K
const ϵ = T * kb # J
const σ = 3.4e-10 # m
const ρ = 1374 # kg/m^3
const m = 39.95 * 1.6747 * 1e-27 # kg
const N = 125
const L = (m*N/ρ)^(1/3)
const R = 2.25σ   
const v_dev = sqrt(kb * T / m) # m/s
const τ = 1e-14  # s
const t1 = 0τ
const t2 = 100τ
{% endhighlight %}
It is up to a user to define initial coordinates and velocities of particles. Nevertheless, it is convenient to place atoms in the nodes of a cubic lattice, at the same time satisfying the value of liquid argon density:
{% highlight julia %}
bodies = generate_bodies_in_cell_nodes(N, m, v_dev, L)
jl_parameters = LennardJonesParameters(ϵ, σ, R)
{% endhighlight %}
Boundary conditions are necessary for simulating a bulk media:
{% highlight julia %}
pbc = CubicPeriodicBoundaryConditions(L)
lj_system = PotentialNBodySystem(bodies, Dict(:lennard_jones => jl_parameters));
{% endhighlight %}
For setting up this simulation it is important to pass in its constructor not only time span of the experiment, but also boundary conditions in wich the system will be tested:
{% highlight julia %}
simulation = NBodySimulation(lj_system, (t1, t2), pbc);
result = @time run_simulation(simulation, VelocityVerlet(), dt=τ)
{% endhighlight %}

The following gif shows movements of atoms in liquid argon:
<img src="https://user-images.githubusercontent.com/16945627/41207210-0dd93c96-6d2b-11e8-8ffe-b20e8797e29e.gif" alt="Here should appear a gif of moving bodies" width="550"/>

As was expected, the temperature of the system fluctuates near some value, which depends on initial velocities and locations of particles in the system:
<img src="https://user-images.githubusercontent.com/16945627/41206938-fcbeac98-6d25-11e8-8951-0f59ddc8abca.png" alt="Here should appear a gif of moving bodies"/>

### Other interactions
All tools, that I develop for n-body simulations, are located in [DiffEqPhysics.jl](https://github.com/JuliaDiffEq/DiffEqPhysics.jl) project. The `test` folder contains examples of all physical interactions already implemented via direct force calculations. At the moment of writing this post, the following interactions can be simulated:
- gravitational
- electrostatic
- magnetostatic
- based on the [Lennard-Jones potential](https://en.wikipedia.org/wiki/Lennard-Jones_potential)
- custom pair-wise interaction implemented via the `CustomAccelerationSystem` interface


