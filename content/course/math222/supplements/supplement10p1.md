---
title: Supplement to Section 10.1 of Boyce & Diprima
linktitle: Supplement to 10.1
toc: false
type: book
date: "2020-02-26"
draft: false


# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 310
---


Section 10.1 of the textbook says a lot about the question of whether or not linear boundary value problems have solutions and whether those solutions are unique, but it doesn't actually tell you how to solve them. Fortunately, you have already seen all the mathematical ideas you need: a little bit of linear algebra, and, for inhomogeneous problems, the methods of undetermined coefficients and variation of parameters

#### Homogeneous problems

Suppose we want to solve the homogeneous boundary value problem (BVP)
$$
\begin{aligned}
\frac{d^2 y}{dx^2}(x)+p(x)\frac{dy}{dx}(x)+q(x)y(x)&=0; \quad 0<x<L; \\\\
y(0) = a; \quad y(L)&= b.
\end{aligned}
$$
We further suppose that we have a fundamental set of solutions to the ordinary differential equation $\{y_1(x),y_2(x)\}$. We know that the general solution is thus given by
$$
y(x)= c_1 y_1(x) + c_2 y_2(x).
$$
We know that we can solve the _initial value problem_ this way, as long as $W(y_1,y_2)(x)\neq 0$. Is the same true for the _boundary value problem?_

Let's try. The boundary conditions can be written:
$$
\begin{gathered}
y(0)=c_1 y_1(0)+c_2 y_2(0)=\alpha;\\\\
y(L)=c_1 y_1(L)+c_2 y_2(L)=\beta.
\end{gathered}
$$
We've seen something very similar to this when we were studying the initial value problem. See the discussion in Section 3.2 of Boyce and diPrima, especially equation $(8)$ on page 112, as well as the surrounding discussion.

This can be rearranged into a vector equation
$$
\begin{pmatrix}
y_1(0) & y_2(0) \\\\
y_1(L) & y_2(L)
\end{pmatrix}
\begin{pmatrix}
c_1 \\\\ c_2
\end{pmatrix}=
\begin{pmatrix}
\alpha  \\\\ \beta
\end{pmatrix} .
$$
Let the $2\times2$ matrix on the left be denoted by $M$. We know such an equation has a unique solution as long as $\det{M}\neq0$. Just as in equation $(11)$ on page 112 of Boyce, this determinant appears in the denominator of the the solution formulas for $c_1$ and $c_2$ so may not vanish unless $\alpha=\beta=0$.

How does this works in practice?

__Example 1__
$$
\begin{aligned}
\frac{d^2 y}{dx^2}(x)+\frac{1}{4}y(x) & = 0;  0<x<\pi \\\\
y(0) =1 ; y(\pi)&=1.
\end{aligned}
$$
This has general solutions $y_1=\cos{\frac{x}{2}}$, $y_2=\sin{\frac{x}{2}}$. For this problem the matrix $M$ is just the identity matrix $M = \left(\begin{smallmatrix} 1 & 0 \\\\ 0 & 1  \end{smallmatrix} \right)$. Therefore the solution is $c_1=1$, $c_2=1$.

__Example 2__

Consider the same problem but let it be defined instead on the interval $0<x<2\pi$. In this case the matrix is $M = \left(\begin{smallmatrix} 1 & 0 \\\\ -1 & 0 \end{smallmatrix}\right)$. The determinant of this matrix vanishes, so the problem is not solvable.

_Can you see why the first example works but the second example fails?_ The solution $y_2$ is zero at both endpoints in the second example, so multiplying it by a constant $c_2$ doesn't change the boundary values!

### Inhomogeneous problems
#### Using undetermined coefficients

In sections 3.5 and 3.6 of Boyce, we learn to solve inhomogeneous second order ODE using the methods of undetermined coefficients and the method of variation of parameters. Recall that these problems take the form
$$
\begin{gathered}
a \frac{d^2 y}{dx^2}(x)+ b \frac{dy}{dx}(x) + c y(x) = g(x); \  0<x<L \\\\
y(0)=\alpha;  y(L)= \beta.
\end{gathered}
$$
We know that the general solution is of the form
$$
y(x)= c_1 y_1(x)+ c_2 y_2(x) + Y(x)
$$
where $y_1$ and $y_2$ satisfy the associated homogeneous problem and $Y(x)$ is any particular solution, which we have learned how to find.

Plugging this solution into the boundary conditions yields
$$
\begin{aligned}
c_1 y_1(0) + c_2 y_2(0) + Y(0) &= \alpha; \\\\
c_1 y_1(L) + c_2 y_2(L) + Y(L) &= \beta.
\end{aligned}
$$
As before, this system can be written as a matrix-vector equation for the unknowns $c_1$ and $c_2$
$$
\begin{pmatrix}
y_1(0) & y_2(0) \\\\
y_1(L) & y_2(L)
\end{pmatrix}
\begin{pmatrix}
c_1 \\\\ 
c_2
\end{pmatrix}=
\begin{pmatrix}
\alpha -Y(0) \\\\
 \beta - Y(L)
\end{pmatrix} ,
$$

and the solvability of this system depends on whether the matrix on the left-hand side has zero determinant.

##### Example

Now we solve the same problem but with a nonhomogeneous term:
$$
\begin{gathered}
\frac{d^2 y}{dx^2}(x)+\frac{1}{4}y(x)  = x+1; \ 0<x<\pi \\\\
y(0) =1 ; y(\pi)=1.
\end{gathered}
$$

For this problem, it's easy enough to solve for $Y(x)$ using undetermined undetermined coefficients, which yields $Y(x)=4x+4$. Therefore, the general solution is
$$
y(x) = c_1 \cos{\frac{x}{2}}+ c_2 \sin{\frac{x}{2}} + 4x + 4.
$$
To solve the BVP we simply have to choose $c_1$ and $c_2$ so that this solution satisfies the boundary conditions. Evaluating the function $y(x)$ at the endpoints we find
$$
\begin{aligned}
y(0) & = c_1 + 4 = 1, \\\\
y(\pi) & = c_2 + 4 \pi + 4 = 1.
\end{aligned}
$$
which gives $c_1= -3$ and $c_2 = -4 \pi -3$.

#### Variation of parameters

The undetermined coefficient method only works for constant coefficient problems where the inhomogeneous term takes a particular form. When it fails, we can use the variation of paramters method.

Consider the inhomogeneous boundary value problem (BVP)
$$
\begin{aligned}
\frac{d^2 y}{dx^2}(x)+p(x)\frac{dy}{dx}(x)+q(x)y(x)&=g(x);\  0<x<L; \\\\
y(0) = \alpha; \  y(L)&= \beta.
\end{aligned}
$$
Recall that the variation of parameters method shows that the general solution to this problem is
$$
y(x)= u_1(x)y_1(x)+u_2(x)y_2(x),
$$
where
$$
u_1'(x) = \frac{-g(x)y_2(x)}{W(y_1,y_2)(x)},
 \text{ and }
 u_2'(x) = \frac{g(x)y_1(x)}{W(y_1,y_2)(x)}.
$$
We can write the solution of these problems as
$$
u_1(x) = c_1 - \int_0^x \frac{g(s)y_2(s)}{W(y_1,y_2)(s)}ds
 \text{ and }
 u_2(x) = c_2 + \int_0^x \frac{-g(s)y_1(s)}{W(y_1,y_2)(s)}ds.
$$
Therefore the solution is
$$
y(x) = \left(c_1 - \int_0^x \frac{g(s)y_2(s)}{W(y_1,y_2)(s)}ds\right)y_1(x)+
\left(c_2 + \int_0^x \frac{g(s)y_1(s)}{W(y_1,y_2)(s)}ds \right) y_2(x).
$$
 The boundary conditions reduce to
$$
 \begin{aligned}
y(0) & = c_1 y_1(0) + c_2y_2(0) = \alpha ;\\\\
y(L) & = \left(c_1 - \int_0^L \frac{g(s)y_2(s)}{W(y_1,y_2)(s)}ds\right)y_1(L)+
\left(c_2 + \int_0^L\frac{g(s)y_1(s)}{W(y_1,y_2)(s)}ds \right) y_2(L)= \beta.
 \end{aligned}
$$
 In matrix vector notation, this is
$$
 M \begin{pmatrix}
 c_1 \\\\ c_2
 \end{pmatrix}=
 \begin{pmatrix}
 \alpha \\\\
 \beta  +
 y_1(L) \int_0^L \frac{g(s)y_2(s)}{W(y_1,y_2)(s)}ds -
 y_2(L) \int_0^L \frac{g(s)y_1(s)}{W(y_1,y_2)(s)}ds
  \end{pmatrix} .
$$
So the condition for the existence of a unique solution is the same as for the homogeneous problem, that $M$ be an invertible matrix.


 ### Other boundary conditions
In the above problem, our boundary conditions depended on the values of the function $y$ at each of the ends. This is called a _Dirichlet_ problem. Other times, we may be interested in the value of $\frac{dy}{dx}$ at one or both ends. This is called a _Neumann_ problem.

### Exercises
1. Solve
$$
\begin{gathered}
\frac{d^2 y}{dx^2} + 4 \frac{dy}{dx} + 3 y = 0,  0<x<1; \\\\
y(0)= 1 ; \ 
y(1)= 2
\end{gathered}
$$

1. Solve
$$
\begin{gathered}
\frac{d^2 y}{dx^2} +  y  = x^2,  0<x<1 \\\\
y(0)= 1 ; \ 
y(1)= 2
\end{gathered}
$$
Note that undetermined coefficients is sufficient for this problem. A sometimes useful formula for the inverse of a $2\times2$ matrix is
$$
\begin{pmatrix}
a & b \\\\ 
c& d\end{pmatrix}^{-1}
=
\frac{1}{ad-bc}
\begin{pmatrix}
d & -b \\\\ 
-c& a\end{pmatrix},
$$
assuming that this determinant is non-zero.

1. Consider this last example again, except place the right-hand boundary condition at a point $x=L$ rather than $x=1$. For which values of $L$ does the problem have a unique solution?
1. Here is one that requires the variation of parameters method:
$$
\begin{gathered}
 2 x^2 \frac{d^2 y}{dx^2} + 3x \frac{dy}{dx} -y = x, 1<x<4 \\\\
 y(1)=5; \  y(4) = 4.
\end{gathered}
$$
To get you started, note that the two homogeneous solutions are $y_1 = x^{-1}$ and $y_2 = \sqrt{x}$.

## Solutions to Exercises

Solutions will be posted here after the due date.
{{% staticref "math222/solution10p1.pdf" %}} My solution (pdf). {{% /staticref %}}  

