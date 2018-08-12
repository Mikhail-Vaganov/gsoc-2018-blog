---
layout: post
title:  "GSoC 2018 - the Final Report"
date:   2018-08-11 23:00:00 +0500
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/latest.js?config=TeX-MML-AM_CHTML">
</script>

During molecular dynamics simulations it is often important to keep the temperature of the system constant.
Different algorithms have been developed for thermostating during the last weeks of July.

The instantaneous temperature of a system of N particles can be calculated using value of kinetic energy:

$$\sum_{i=1}^{N}\frac{|\textbf{p}_i|^2}{2m_i}=(3N-N_{c})\frac{k_bT}{2},$$

where $$\textbf{p}_i$$ denotes momentum of the i-th particle and $$m_i$$ is its mass, $$N_c$$ is the number of constraints and $$3N-N_c$$ is the total number of degrees of freedom, $$k_b$$ is the Boltzmann constant and $$T$$ is the desired temperature.

For instance, a molecule of water has $$N=3$$ atoms and $$N_c=2$$ bonds connecting atoms of hydrogen with one atom of oxygen, so the number of degrees of freedom for this molecules is $$N_{df}=7$$.
If a system for modelling water contains 1000 molecules, then $$N=3000$$, $$N_c=2000$$ and $$N_{df}=7000$$.

All the figures of the temperature control in this post are presented for the simulation results of the water SPC/Fw model. 

### Andersen thermostat

The Andersen thermostat doesn't keep the energy of a system constant, so it is used for equilibration and initial stabilization of the temperature in microcanonical ensembles (NVE). But it can be used for simulation of a canonical ensemble (NVT).
Also the Andersen thermostat can be used only for time-independent properties.
At every integration timestep, velocity of random particles is rescaled according to the Maxwell–Boltzmann statistics for the given temperature.
Those random particles are chosen according to the Poisson distribution:

$$P(t)=\nu e^{-\nu t},$$

where $$\nu$$ is the stochastic collision frequency.
Actually, I have got a post devoted to the [Andersen thermostat](https://mikhail-vaganov.github.io/gsoc-2018-blog/2018/06/25/andersen.html)

<img src="https://user-images.githubusercontent.com/16945627/43805529-8c2193f8-9ab9-11e8-90e3-a2eb166ee884.png" alt="picture for the Andersen thermostating">

### Berendsen thermostat
The idea behind the Berendsen thermostat is to modify the Langevin equation of
motion in the sense of removing the local temperature coupling through
stochastic collisions (random noise), while retaining the global coupling (principle
of least local perturbation). This can be represented in the following equation:

$$\frac{dT}{dt}=\frac{1}{\tau_B}\Big[T_0-T(t)\Big]$$

The global additional temperature coupling is accomplished by the equations

$$m_i\frac{dv_i}{dt}=F_i+m_i\gamma \Big(\frac{T_0}{T(t)}-1\Big)v_i$$

Here I present results of thermostating of systems for liquid argon and water:

If the $$\tau_B->0$$ and in practice when $$\tau_B=dt$$, the Berendsen thermostat turns into the Woodcock thermostat, which does not allow for temperature fluctuations. 

<img src="https://user-images.githubusercontent.com/16945627/43805536-97827a00-9ab9-11e8-971d-9c093e27b923.png" alt="picture for the Berendsen thermostating">

### Nosé-Hoover thermostat
For the original Nosé algorithm the idea was to extend the real system by addition of an artificial
 $$(N_{df}+1)^{th}$$ dynamical variable $$\zeta$$ associated with a mass $$Q>0$$ and having the velocity $$\frac{d\zeta}{dt}$$.
 So the system Hamiltonian is extended by introducing a thermal reservoir and a friction term in the equations of motion. 
 The friction force is proportional to the product of each particle's velocity and a friction parameter, $$\zeta$$

 In terms of real system variables the Lagrangian equations of motion can be written as
 
 $$\frac{d^2\textbf{r}}{dt^2}=\frac{\textbf{F}_i}{m_i}-\zeta\frac{d\textbf{r}}{dt}$$
 
 $$\frac{d\zeta}{dt}=\frac{1}{\tau_{NH}}\Big(\frac{g}{N_{df}} \frac{T_0}{T(t)}-1  \Big)$$
 
 with the effective relaxation time
 
$$\tau^2_{NH}=\frac{Q}{N_{df}k_BT_0}$$

where $$g=N_{df}$$ in real-time sampling (Nose-Hoover formalism) and $$g=N_{df}+1$$ for virtual-time sampling (Nose-formalism).

<img src="https://user-images.githubusercontent.com/16945627/43805545-9e40a27c-9ab9-11e8-8a06-d999a8fbe2be.png" alt="picture for the Nose-Hoover thermostating">

### Langevin thermostat

In Langevine dynamics the temperature is maintained by modifying the Newton's equations of motion 
with a friction defined by the coefficient $$\gamma$$ and a random force with dispersion $$\sigma=\sqrt{2\gamma k_BT_0}$$

$$m_i\frac{d^2\textbf{r}}{dt^2}=F_i-\gamma\frac{dr}{dt}+\sqrt{2\gamma k_BT_0}R(t),$$

where $$R(t)$$ is the Wiener process.

<img src="https://user-images.githubusercontent.com/16945627/43805836-bb3c3642-9aba-11e8-9fe8-fba188e56098.png" alt="picture for the Langevin thermostating">

### Code
Implementation of all the thermostats can be examined in [NBodySImulation.jl](https://github.com/JuliaDiffEq/NBodySimulator.jls) project and inside the corresponding pull request [#5](https://github.com/JuliaDiffEq/NBodySimulator.jl/pull/5)