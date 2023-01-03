---
title: "Project 4 Solution:"
linktitle: "🤧 Project 4 solutions"
toc: true
type: book
draft: true
weight: 245
---
### Part 1: Applying Euler Method to SIR Model

* {{% staticref "math222f21/SIRsolution.html" %}} MATLAB-generated webpage{{% /staticref %}} containing my solution. 
* {{% staticref "math222f21/SIRsolution.mlx" %}} MATLAB live script{{% /staticref %}} from which it was generated. 

### Part 2: Phase plane study of SIR Model

We can factor the right hand side the differential equation describing the rate of change of the infected population. $\frac{dI}{dt}=\beta S I - \gamma I = (\beta S - \gamma)I$. We see that it is only zero if $I=0$, i.e., when the infection has died out, or when $\beta S = \gamma$ which we can solve to get $S_{\rm critical}=\gamma/\beta$. 

We include here three images of a phase plane, for the same three sets of parameters that we used in our solution to part 1. In addition to plotting the $S-I$ phase plane, I have plotted the nullcline where $\frac{dI}{dt}$ vanishes.

In all three figures $\gamma=1$, and we have chosen the three values $I=0.0002$, $I=0.000125$ and $I=0.00009$. These place the nullclines at the vertical lines $S_{\rm critical} = 5000$, $S_{\rm critical} =8000$, and $S_{\rm critical} \approx 11111$. Note that the infected population is growing as long as $S>S_{\rm critical}$.

Note we have assumed a population $N=10000$. As we decrease $\beta$, the critical value of $S_{\rm critical}$ increases, and therefore.

### Case 1, $\beta=0.0002$

The nullcline is right in the middle of the phase plane. The total infected population rises a lot before falling.

{{< figure library="true" src="math222/SIR_phaseportrait1.png" title="Phase portrait for $\beta=0.0002$ " lightbox="true" >}}

### Case 2, $\beta=0.000125$

The nullcline has moved further to the right, so that the infection rate does not reach such a high level and in fact begins decreasing while there a lot more susceptible people, so that fewer people get sick.

{{< figure library="true" src="math222/SIR_phaseportrait2.png" title="Phase portrait for $\beta=0.000125$ " lightbox="true" >}}

### Case 3, $\beta=0.00009$

Now the nullcline lies to the right of our total population, i.e. $S_{\rm critical}>10000$, so that no matter what the initial number of infected people, $\frac{dI}{dt}<0$ from the beginning and the outbreak is averted.

{{< figure library="true" src="math222/SIR_phaseportrait3.png" title="Phase portrait for $\beta=0.00009$ " lightbox="true" >}}

### What about vaccination?

We see that no matter what the current state of the epidemic, vaccinating some population $P$ is equivalent to immediately taking $S\to S-P$. Suppose at the current time $S > S_{\rm critical}$  so that the rate of infections is still increasing. hen if we can vaccinate a population of size $P > S - S_{\rm critical} $, then this moves the current position to the left of the nullcline and number of infections will begin to decrease. 

