---
# Course title, summary, and position.
linktitle: Math 473 
summary: Fall 2020
weight: 473

# Page metadata.
title: Math 473 Intermediate Differential Equations
date: "2020-12-07T00:00:00Z"
lastmod: "2020-12-07T00:00:00Z"
draft: false # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: book  # Do not modify.
tags: 
- previous

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
#menu:
#  math473:
#    name: Math 473
#    weight: 1
---
### Course Info
Almost everything is on the [course canvas page](https://njit.instructure.com/courses/13893).

### MATLAB Info

I put a little bit of MATLAB stuff here, but it is also mainly on Canvas.

#### MATLAB startup file
I use a file [`startup.m`](https://www.mathworks.com/help/matlab/ref/startup.html) to replace the default MATLAB graphing settings with something a little bit nicer. In particular, I include $\LaTeX$ formatting in my axis labels. $\LaTeX$ is a markup language that mathematicians use to typeset math. If any of my MATLAB programs give you errors, run these lines first:
```matlab
set(groot,'DefaultAxesFontSize',14)
set(groot,'DefaultLineLineWidth',2)
set(groot,'DefaultFunctionLineLineWidth',2)
set(groot,'DefaultTextInterpreter','latex')
set(groot,'DefaultAxesTickLabelInterpreter','latex')
set(groot,'DefaultLegendInterpreter','latex');
```

#### Add-on software
In Part 1 of _Strogatz_ it will be helpful to use software to draw direction fields. In Part 2, it will be useful to use software to draw phase planes. This is not built into MATLAB in a simple way, so you'll have to install some [additional software](https://github.com/MathWorks-Teaching-Resources/Phase-Plane-and-Slope-Field). The site provides complete documentation and how-to videos.


### Here are some HTML files exported from MATLAB

I have created a few tutorials that we will be using over the course of the semester. They will be linked from Canvas, but I am collecting them here for reference:

#### Basic MATLAB Usage

  * **First MATLAB assignment:** Quantitative Comparison of Euler, Improved Euler, \& Runge-Kutta Methods $\bullet$ {{< staticref "math473/matlab1.html">}}HTML{{< /staticref >}}  $\bullet${{< staticref "math473/matlab1.mlx">}} 📄 Live Script{{< /staticref >}}
    
      * Solution: $\circ$ {{< staticref "math473/matlab1solution.html">}}HTML{{< /staticref >}} $\circ${{< staticref "math473/matlab1solution.mlx">}} 📄 Live Script{{< /staticref >}}
      
  * **MATLAB Basics:** Function Handles and Anonymous Functions $\bullet${{< staticref "math473/functionHandle.html" >}}HTML{{< /staticref >}} $\bullet${{< staticref "math473/functionHandle.mlx">}} 📄 Live Script{{< /staticref >}}
  
      * Solution: $\circ$ {{< staticref "math473/functionHandleSolution.html">}}HTML{{< /staticref >}} $\circ${{< staticref "math473/functionHandleSolution.mlx">}} 📄 Live Script{{< /staticref >}}

  * **MATLAB Basics:** Using `ode45` $\bullet$ {{< staticref "math473/ode45tutorial.html" >}}HTML{{< /staticref >}} $\bullet${{< staticref "math473/ode45tutorial.mlx">}}  📄 Live Script{{< /staticref >}}

      * Problem 1 solution: $\circ$ {{< staticref "math473/ode45Problem1.html">}}HTML{{< /staticref >}} $\circ${{< staticref "math473/ode45Problem1.mlx">}} 📄 Live Script{{< /staticref >}}
      
      * Problem 2 solution: $\circ$ {{< staticref "math473/ode45Problem2.html">}}HTML{{< /staticref >}} $\circ${{< staticref "math473/ode45Problem2.mlx">}} 📄 Live Script{{< /staticref >}}

### Companion to the textbook

  * **A Simple Bifurcation Diagram**: A MATLAB based solution to Strogatz problem 4.3.3. $\bullet${{< staticref "math473/strogatz4p3p3.html" >}}HTML{{< /staticref >}} $\bullet${{< staticref "math473/strogatz4p3p3.mlx">}} 📄 Live Script{{< /staticref >}}

  * **The Lorenz system:** Companion to Chapter 9 $\bullet${{< staticref "math473/lorenzDemo.html" >}}HTML{{< /staticref >}} $\bullet${{< staticref "math473/lorenzDemo.mlx">}} 📄 Live Script{{< /staticref >}}

  * **Iterated Map Demonstration:** Companion to Strogatz 10.1--10.4 {{< staticref "math473/mapDemo.html" >}}HTML{{< /staticref >}} $\bullet${{< staticref "math473/mapDemo.zip">}} 📄 Live Script and supporting files{{< /staticref >}}

  * **The Rössler system:** Companion to Strogatz 10.6 $\bullet${{< staticref "math473/rosslerDemo.html" >}}HTML{{< /staticref >}} $\bullet${{< staticref "math473/rosslerDemo.mlx">}} 📄 Live Script{{< /staticref >}}

* **Box-counting dimension for the Hénon attractor:** Companion to Strogatz 11.4 $\bullet${{< staticref "math473/dimensionDemo.html" >}}HTML{{< /staticref >}} $\bullet${{< staticref "math473/dimensionDemo.mlx">}} 📄 Live Script{{< /staticref >}}

* **Attractor reconstruction for the Lorenz System** (similar to Strogatz 12.4) $\bullet${{< staticref "math473/lorenzReconstruct.html" >}} HTML {{< /staticref >}} $\bullet${{< staticref "math473/lorenzReconstruct.mlx">}} 📄 Live Script{{< /staticref >}}

* **Attractor reconstruction for a chemistry experiment:**  Companion to Strogatz 12.4 $\bullet${{< staticref "math473/BZReconstruct.html" >}} HTML {{< /staticref >}} $\bullet${{< staticref "math473/BZReconstruct.mlx">}} 📄 Live Script{{< /staticref >}} $\bullet${{< staticref "math473/BZdata.txt">}} 📄 Data File{{< /staticref >}}

* **Strange attractor for the chaotic forced-damped double well oscillator:**  Companion to Strogatz 12.5, following Moon \& Holmes 1979 $\bullet${{< staticref "math473/doubleWellExample.html" >}} HTML {{< /staticref >}} $\bullet${{< staticref "math473/doubleWellExample.mlx">}} 📄 Live Script{{< /staticref >}}

* **Forced-damped pendulum example**: Shows period-doubling in a Poincaré map of the forced damped pendulum $\bullet${{< staticref "math473/pendulumExample.html" >}}HTML{{< /staticref >}} $\bullet${{< staticref "math473/pendulumExample.mlx">}} 📄 Live Script{{< /staticref >}}

     