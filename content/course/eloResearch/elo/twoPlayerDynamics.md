---
title: Two Player Dynamics 👫
linktitle: Two Player Dynamics
toc: true
type: book
date: 2026-02-08
draft: false
tags:
    - unsolved
    - elo

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 6101
---

## Problem Description

Let us focus on the scenario in which we are dealing only with two players $T=\left\\{x,y\right\\}$ and want to run Elo in order to see what their predicted skill levels are.

We will assume that the players' skills are fixed, and thus there exists some value $p$ such that
$$
Pr(x \text{ wins over } y) = p 
$$
and by extension
$$
Pr(y \text{ wins over } x) = 1-p 
$$
which in our matrix is
$$
P=\begin{bmatrix}
0 & p \\\\
1-p & 0
\end{bmatrix}
$$

We can completely parameterize our system by observing the gap value of $r=x-y$, instead of observing the sequence of Elo ratings $x$ and $y$. 

Since our update rules in SGD would have been
$$
\begin{align}
x_{t+1}&=\begin{cases}
x_t + \eta\cdot b(y_t-x_t) &\text{ if x wins} \\\\
x_t - \eta\cdot b(x_t-y_t) &\text{ if y wins} 
\end{cases} \\\\
y_{t+1}&=\begin{cases}
y_t + \eta\cdot b(x_t-y_t) &\text{ if y wins} \\\\
y_t - \eta\cdot b(y_t-x_t) &\text{ if x wins} 
\end{cases}
\end{align}
$$
which if we subtract to get $r_{t+1}=x_{t+1}-y_{t+1}$ then we get that
$$
r_{t+1}=\begin{cases}
r_t + 2\eta\cdot b(-r_t) &\text{ if x wins} \\\\
r_t - 2\eta\cdot b(r_t) &\text{ if y wins} 
\end{cases}.
$$

From here we have entered into a $1$-variable SGD dynamic, however the interesting aspect is that the data that we sample will select between the functions
$$
r_t\pm 2\eta\cdot b(\pm r_t)
$$
with weighted probability $p$ instead of $\frac{1}{2}$! 

Using algebra, we also can see what we expect the "true" skill gap between the two players to be
$$
p = \frac{1}{1+e^{-r}} \implies R = -\log\left(\frac{1}{p}-1\right),
$$
and since we are centering around the origin, then $\vec{\rho}=\left(\frac{R}{2}, -\frac{R}{2}\right)^T$.

> **Problem**: Can we classify the dynamics of Elo for various values of $\eta, p$? Does it always converge to $\vec{\rho}$ after sufficiently long? Is this a better or faster predictor of $p$ than proportion estimation?

---

## Random Maps and their Inverses

Similar to previous work we've done on SGD, we will represent our process as the following functions
$$
\begin{align}
\varphi_1(r) &= r + \frac{2\eta}{1+e^r} \\\\
\varphi_2(r) &= r - \frac{2\eta}{1+e^{-r}}
\end{align}
$$
where we select $\varphi_1(r)$ with probability $p$ and $\varphi_2(r)$ with probability $1-p$. We now want to calculate the inverse maps of these functions so we can start performing methods of numerical computation on them

### Computation of Inverse Maps

For simplification purposes, we will let $k=2\eta$ and $s=\frac{1}{1+e^{-r}}=\frac{e^r}{1+e^r}$[^1]. This means that
$$
r=\log\left(\frac{s}{1-s}\right)
$$
and thus our second map[^2] is
$$
\varphi_2(r) = \log\left(\frac{s}{1-s}\right)-ks
$$
Letting $y=\varphi_2(r)$ we have that
$$
\begin{align}
y &= \log\left(\frac{s}{1-s}\right)-ks \\\\
e^y &= \frac{s}{1-s}e^{-ks} \\\\
-e^y &= e^{-ks}\frac{s-0}{s-1}
\end{align}
$$
this provides us with the form to use the $r$-Lambert $W$ function[^3] to get our inverse with $t=0$, $s=1$, $c=-k$, $a=-e^y$, $T=t-s=-1$
$$
s = -\frac{1}{k}W_{e^y}\left(-ke^y\right)
$$
replacing all of our variables and solving accordingly we have that
$$
\begin{align}
\frac{1}{1+e^{-r}}&=-\frac{1}{2\eta}W_{e^y}\left(-2\eta e^y\right) \\\\
r&=-\log\left(-\frac{2\eta}{W_{e^y}\left(-2\eta e^y\right)}-1\right)
\end{align}
$$
so our inverse map for the purposes of use in Ulam's method will be
$$
\varphi_2^{-1}(x) = -\log\left(-\frac{2\eta}{W_{e^x}\left(-2\eta e^x\right)}-1\right),
$$
in which we use $x$ just to avoid variable name confusion, however this will be $r$ when applied for Ulam's method.

In order to solve for $\varphi_1^{-1}$ we introduce the change of variables $r'=-r$ and $y'=-y$ so
$$
y' = r' - \frac{2\eta}{1+e^{-r'}}
$$
which gives us our same inverse as before
$$
r'=-\log\left(-\frac{2\eta}{W_{e^{y'}}\left(-2\eta e^{y'}\right)}-1\right)
$$
and thus
$$
\varphi_1^{-1}(x)=\log\left(-\frac{2\eta}{W_{e^{-x}}\left(-2\eta e^{-x}\right)}-1\right).
$$

[^1]: Change of variables was suggestion of ChatGPT which proved quite useful
[^2]: We are starting with the second map so that we can work with $e^{-r}$ first. We will perform a change of variables afterwards to get the first map into the same form.
[^3]: https://www.ams.org/journals/tran/2017-369-11/S0002-9947-2017-06911-7/S0002-9947-2017-06911-7.pdf