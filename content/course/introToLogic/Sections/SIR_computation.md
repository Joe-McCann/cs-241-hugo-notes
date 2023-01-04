---
title: "Project 4: SIR Models of Epidemics: Computation"
linktitle: "Project 4: SIR Models"
toc: true
type: book
draft: false
weight: 240
---


## What to turn in

Write your results in a word processor and export the final result into a PDF to upload. It should be divided into two parts, with headings **Part 1** and **Part 2** corresponding to the two sections below.

### Part 1

* This {{% staticref "math222f21/euler2d.html" %}}MATLAB-generated webpage{{% /staticref %}} demonstrates how to use Euler's method to solve a two-component ODE system. Download the {{% staticref "math222f21/euler2d.mlx" %}}live script{{% /staticref %}} and edit the program to solve instead the differential equations satisfied by $S(t)$, $I(t)$, and $R(t)$ in [the SIR supplement](../../supplements/sir_modeling).
* Use this MATLAB program to explore the model: play around with initial conditions and parameters and see what happens in the simulations (a more systematic analysis will be performed later). Observe the effect of each parameter and the possible courses of simulated epidemics. Is it possible to simulate a sustained epidemic in this model, i.e., one in which $I(t)$ does not eventually go to zero.
* Turn in your code, any outputs (graphs) and a few sentences explaining what you notice.

This definition of the __Reproductive ratio__ should guide your thinking;

__The Reproductive Ratio $R_0$:__ A key parameter in epidemiology is the basic reproductive ratio, $R_0$. It is defined as the average number of secondary cases transmitted by a single infected individual that is placed into a fully susceptible population. In other words, $R_0$ tells us about the initial rate of spread of the disease. Hence, if $R_0 > 1$, there will be an epidemic, and if $R_0 < 1$, the introduced infecteds will recover (or die) without being able to replace themselves by new infections. In this model, it is pretty easy to derive $R_0$. The disease-free state corresponds to: $S=N$, $I=0$, $R=0$. If one infected individual appears in the population, there will be an epidemic if and only if $\frac{dI}{dt} > 0$. By replacing $S$ with $N$ in the $\frac{dI}{dt}$ equation, this yields  $\frac{N \beta}{\gamma} > 1$. That is:
$$ R_0 = \frac{\beta N}{\gamma}.$$ Check this formula by simulating the model for different sets of parameters. Fix $N$, and vary $\beta$ and $\gamma$. Choose your values such as to have combinations with both $R_0 > 1$ and $R_0 < 1$. 

[^3]:MATLAB hint: Using the equality operator == compares two matrices of equal size element by element, and yields a logical matrix (a matrix whose entries are all logical true (1) or false (0)) of the same size as a result. In contrast, the function __isequal()__ checks wheter two matrices are identical and returns a single logical value.

### Part 2

This section is a bit more open-ended.  

Phase portraits (or phase diagrams) provide a powerful tool to visualize the dynamics of ODE systems. Use the [Phase Plane App](https://github.com/MathWorks-Teaching-Resources/Phase-Plane-and-Slope-Field) to compute the phase planes necessary for this part of the assignment.

Notice that the basic SIR model can be reduced to a two-dimensional system, because the variable $R(t)$ for recovered individuals does not appear in the evolution equations for the other two variables. The reduced $(S,I)$ system is thus the following:
$$
\begin{aligned}
\frac{\mathrm{d}}{\mathrm{d}t} S & = -\beta S I \\\\
\frac{\mathrm{d}}{\mathrm{d}t} I & = \beta S I - \gamma I
\end{aligned}
$$
and can be plotted in a 2D phase diagram without loss of information. For a fixed set of parameters, draw a phase portrait with a few trajectories corresponding to different initial conditions. Observe how the course of the epidemic depends on the initial conditions. Draw trajectories for different values of $N$. For each simulation, identify the maximum value of $I$, and draw the point on the phase diagram. 

It is possible to determine analytically the value of $S$ for which the epidemic reaches its peak. Using $R_0$, we find  $S_{I \rm{\ is\ max}} = \frac{\gamma}{\beta}$. Plot these values on your phase diagram, and compare them to the points you obtained previously. Interpret the result. In particular, what
happens when $S_0 \le \gamma/\beta$?

__The Vaccine__ Now that we have a vaccine, we can think about how vaccinating a portion of the population helps protect even the unvaccinated. Suppose that one morning there are $S$ susceptible and $I$ infected in the population and that day $V<S$ of them get vaccinated. You can think of this as $S\to S-V$ and $R\to R+V$, while $I$ remains unchanged. How does that change things? Can you use the phase plane plot to help your reasoning?

---
For more information, see this nice article[^2]. [+plus Magazine](https://plus.maths.org/content/tags/covid-19) has a large collection of undergraduate-level articles on mathematics useful in understanding various aspects the COVID-19 pandemic. Another good resource comes from the [American Mathematical Society](http://www.ams.org/home/covid-19), see under the _Mathematical Modeling_ subheading.

[^1]: Kermack, W. O. and McKendrick, A.G. (1927) [Contribution to the matimatical theory of epidemics--1.](https://royalsocietypublishing.org/doi/10.1098/rspa.1927.0118), _Proc. Roy. Soc._ __115A__, 700.

[^2]: Keeling, M. (2001) [The Mathematics of Diseases](https://plus.maths.org/content/os/issue14/features/diseases/index), +plus magazine