---
# Course title, summary, and position.
linktitle: Math 676
summary: Fall 2022
weight: 676

# Page metadata.
title: Math 676 Graduate Differential Equations

draft: false # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: book  # Do not modify.
tags: 
    - current

---


### Course Info

Almost everything is on the [course canvas page](https://njit.instructure.com/courses/25954/) or on the official Math Department [syllabus](https://docs.google.com/document/d/1OdW1ayElsS67pVct_RhMLtWu22ovh7k_ewEhhuUq9Z8). Canvas and Google Docs are bad at rendering mathematics and computer code, so I maintain this webpage to post math-heavy and code-heavy information

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
This changes how math is interpreted in figures and requires any $\LaTeX$ math to be enclosed between dollar signs.

#### MATLAB Dynamical Systems Software
At some point, we may want to draw direction fields and phase planes. This capability is not built into MATLAB in a simple way, so you'll have to install some [additional software](https://github.com/MathWorks-Teaching-Resources/Phase-Plane-and-Slope-Field). The site provides complete documentation and how-to videos.

#### Computational solutions to homework problems
* {{< staticref "math676/patrick3p4c.html" >}}Meiss 3.4c{{< /staticref >}}
* [A publication]({{< relref "/publication/FPUT_MiM" >}})

#### Old Lecture Notes
These are lecture notes from an earlier iteration of the course. They mostly match what I do now:


* {{< staticref "math676/Lectures/Lecture1.pdf" >}}Lecture 1 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture2.pdf" >}}Lecture 2 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture3.pdf" >}}Lecture 3 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture4.pdf" >}}Lecture 4 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture5.pdf" >}}Lecture 5 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture6.pdf" >}}Lecture 6 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture7.pdf" >}}Lecture 7 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture8.pdf" >}}Lecture 8 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture9.pdf" >}}Lecture 9 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture10.pdf" >}}Lecture 10 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture11.pdf" >}}Lecture 11 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture12.pdf" >}}Lecture 12 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture13.pdf" >}}Lecture 13 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture14.pdf" >}}Lecture 14 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture15.pdf" >}}Lecture 15 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture16.pdf" >}}Lecture 16 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture17.pdf" >}}Lecture 17 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture18.pdf" >}}Lecture 18 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture19.pdf" >}}Lecture 19 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture20.pdf" >}}Lecture 20 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture21.pdf" >}}Lecture 21 {{< /staticref >}} 
* {{< staticref "math676/Lectures/Lecture22.pdf" >}}Lecture 22 {{< /staticref >}} 
