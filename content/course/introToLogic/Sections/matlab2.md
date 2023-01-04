---
title: MATLAB Project 2
linktitle: Project 2, Slope field
toc: false
type: book
date: "2021-01-05"
draft: false

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 215
---

__NJIT Math 222__ 

### Problem 1

Consider the differential equation
$$
\frac{dx}{dt}= f(x) = \sin{x}-a \sin{4x}
$$
This equation has period  $2\pi$, so we will consider it for $-\pi\le x \le \pi. $ Things will be clearer if we plot instead on a slightly larger interval such as $-4\le x \le 4$. When $a=0$, this equation has two equilibria per period, at $x=0$ and $x=\pi$ (considering $x=\pm\pi$ as the same point). When $a$ is large (say $a=4$) then the  graph of $f(x)$ looks pretty much like $-a\sin{4x}$ (mathematicians would say the second term dominates), and the system has 8 fixed points per period.

Starting with $a=0$ plot the direction field for several increasing values of $a$. You should find that at a certain value of $a$ satisfying  $0.2<a<0.3$, two new equilibria appear and at a second critical value satisfying $0.8<a<1$, four more equilibria appear. 

Print out enough graphs to describe what happens in each case and describe what you observe in a sentence or two. Pay attention to the changes in the neighborhood of the equilibrium $x=0$. **Hint:** it is useful to plot $f(x)$ for various values of $a$ as well. The website and apps from [Desmos](https://www.desmos.com) are easy to use and output beautiful graphics.

### Problem 2

Graph the direction field and some solutions for the non-autonomous equation
$$
\frac{dx}{dt}=x - \frac{t x}{1+x^2}.
$$

It is up to you to pick the range over which you plot in $x$ and $t$ in order to see the behavior.

There is a finite number of things that can happen as $t\to\infty$. Describe them.

