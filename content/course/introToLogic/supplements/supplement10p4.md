---
title: Supplement to Section 10.4 of Boyce & Diprima
linktitle: Supplement to 10.4
toc: false
type: book
date: "2020-02-26"
draft: false

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 320
---

## Solving linear boundary value problems using Fourier series

### Example 1
Let's try to solve the simple inhomogeneous boundary value problem
$$
\begin{gathered}
y''(x)  = f(x); 0< x< \pi \\\\\\
y(0) =  y(\pi) = 0.
\end{gathered}
$$

First of all, let's find the eigenfunctions. These solve $y''(x) = \lambda y(x)$. We know from section 10.2 of Boyce that the eigenvalues are negative, so let $\lambda = -\nu^2$. The general solution is $y = a \sin{\nu x} + b \cos{\nu x}$. The boundary conditions then require $b=0$ and $\nu = n=1,2,3,\ldots$ Therefore we look for solutions
$$
y = \sum_{n=1}^\infty c_n \sin{n x}.
$$
So far everything we are doing is straight out of section 10.2. If this part has been difficult, go back there and review.

Then
$$
y''(x) = \sum_{n=1}^\infty -n^2 c_n \sin{n x}.
$$
Now, following section 10.3 of Boyce, we can expand $f(x)$ in Fourier sine series on $0\le x\le \pi$, so that we must use $L=\pi$ in our computations. The formulas give
$$
f(x)= \sum_{n=1}^\infty b_n \sin{n x},
$$
where 
$$
b_n = \frac{2}{\pi}\int_0^{\pi} f(x) \sin{nx}\ dx.
$$

Equating the coefficients of $\sin{nx}$ gives
$$
-n^2 c_n = b_n = \frac{2}{\pi}\int_0^{\pi} f(x) \sin{nx}\ dx
$$
so that the coefficients are
$$
c_n = -\frac{b_n}{n^2}=-\frac{2}{n^2\pi}\int_0^\pi f(x) \sin{nx} \ dx.
$$

If we specify the right-hand side, we may arrive at a closed form solution. For example, we follow Example 1 on page 484,  letting $f(x)=x$ and taking $L=\pi$, then
$$
b_n = \frac{  2(-1)^{n+1}}{n}
$$
and
$$
c_n =
-\frac{2}{n^2} \frac{  (-1)^{n+1}}{n} = \frac{2(-1)^n}{n^3}.
$$
In fact, we can solve this problem exactly using undetermined coefficients and the ideas in the first set of supplementary notes. The exact solution is
$$
y = \frac{1}{6}\left(-\pi^2 x + x^3\right).
$$
The coefficients $c_n$ match those of the Fourier sine coefficients of the odd extension of this function.
### Example 2
Modify the problem slightly to
$$
\begin{gathered}
y''(x)  + c y = f(x); 0< x< \pi \\\\\\
y(0) =  y(\pi) = 0
\end{gathered}
$$
Once again, we find the eigenfunctions are $y_n = \sin{nx}$, but the eigenvalues are $\lambda_n = c-n^2$. Plugging in the same expansion as before, we now find that
$$
\left(c-n^2\right) c_n = \frac{2}{\pi}\int_0^\pi f(x) \sin{nx} \ dx
$$
So if $c=m^2$ for any integer $m$, this equation clearly has no solution. However, as long as this condition is not satisfied, we can solve for the coefficients:
$$
c_n = \frac{2}{\pi \left(c-n^2\right)}\int_0^\pi f(x) \sin{nx} \ dx.
$$
Note that if $c=n^2$ for any positive integer $n$ then the problem has no solution (at least not in the form of a Fourier sine series).

## Exercises

1. Find the Fourier sine-coefficients if we use $f(x)=x$ and $c=-1$ in Example 2. Find the exact solution using the method of undetermined coefficients. Suppose $c=+1$. Then the problem can't be solved unless $b_1=0$. Why not?
1. Modify the above examples to satisfy Neumann boundary conditions, $y'(0)=y'(\pi)=0$. This will give a Fourier cosine series, and will correspond to an even extension. You will find that Example 1 does not yield a solvable problem unless the term $a_0=0$.

## Solutions to Exercises

Solutions will appear here after the due date.

{{% staticref "math222/solution10p4.pdf" %}} My solution (pdf). {{% /staticref %}} 