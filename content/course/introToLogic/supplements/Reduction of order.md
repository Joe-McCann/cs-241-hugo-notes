---
title: "Supplement: Reduction of order Simplified"
linktitle: "Reduction of Order"
toc: true
type: book
draft: false
weight: 305
---


Section 3.4 of the textbook presents the idea of "Reduction of Order" as a way to find the second solution to the problem 
$$ a y'' + b y' + c y = 0  \ 😄 $$ 
in the case that $b^2-4ac=0$ and the characteristic equation only has one unique root $r = -\frac{b}{a}$.

The method it uses to derive reduction of order is the standard one, and I recently learned about an alternate version that is both simpler to derive and leads to simpler calculations from Natalya Tsipenyuk, who is teaching two sections of this class. Simpler calculations are good as they are less prone to user mistakes.

We begin with the general second order linear homogeneous equation
$$
y'' + p(t) y' + q(t)y = 0 \ 🐶.
$$
We recall that Section 3.2 includes Abel's Theorem, which states that if $ y_1(t)$ and $y_2(t)$  solve equation 🐶, then
$$ W(y_1,y_2) = Ce^{-\int p(t) dt} .$$ If, in addition, $y_1$ and $y_2$ form a fundamental solution set, then $C \neq 0$ and, by suitably scaling the functions, we can assume $C=1$.

The big idea of the reduction of order method is to assume that
$$ y_2(t) = y_1(t) v(t)$$
and to derive a _first order_ differential equation for $v(t)$. To do this we use Abel's formula. With this definition of $y_2$, and using the product rule, we have
$$
\begin{aligned}
W(y_1,y_2)= e^{-\int p(t) dt} &= y_1 y_2' - y_1' y_2 \\\\
&= y_1 ( y_1' v + y_1 v') -y_1' y_1 v \\\\
& = (y_1)^2 v'  .
\end{aligned}
$$
Solving this for $v'(t)$ we get
$$
v'(t) = \frac{ e^{-\int p(t) dt}}{y_1(t)^2}.
$$

Let's apply this to some of the examples used in Boyce and DiPrima:

##### Example 3
Given that $y_1(t) = t^{-1}$ is a solution to 
$$ 
2 t^2 y'' + 3ty'- y = 0,
$$
find a fundamental set of solutions. 

First, we put the equation in standard form by dividing by $2t^2$:
$$
y'' + \frac{3}{2t}y' - \frac{1}{2t^2}y = 0
$$
so $p(t) = \frac{3}{2t}$,
$$
\int p(t) dt = \frac{3}{2} \ln{t}
$$
and
$$
 e^{-\int p(t) dt} = t^{-3/2},
$$
yielding
$$
v'(t) 
= \frac{t^{-3/2}}{\left(t^{-1}\right)^2}
= t^{1/2}
$$
and 
$$
v(t) = \frac{2}{3} t^{3/2}+ k.
$$
Therefore we can take 
$$y_2(t) = y_1(t) v(t) = t^{-1} \left(\frac{2}{3} t^{3/2}+ k\right) = \frac{2}{3} t^{1/2}+ k t^{-1}.$$
However, the term $k t^{-1}$ is just $k y_1$ and we can set $k=0$. Further, we can multiply $y_2$ by any nonzero constant and still get a nontrivial solution. In particular, if we multiply it by $\frac32$ we still have a solution. This leaves, finally,
$$
y_2 = t^{1/2}.
$$

### Main example
Let's go back to the equation labeled :smile:. We put it in the standard form
$$
y'' + \frac{b}{a} y' + \frac{c}{a} y = 0
$$
If $b^2-4ac=0$ then $r = -\frac{b}{2a}$ and $y_1 = e^{rt}$. Furthermore, $p(t) = \frac{b}{a}$ so 
$$
e^{-\int p(t)dt} = e^{-\frac{b}{a}t} = e^{2rt}.
$$
Therefore
$$
v'(t) = \frac{e^{-\int p(t)dt}}{y_1(t)^2}= \frac{e^{2rt}}{\left(e^{rt}\right)^2}=1
$$
and 
$$
v(t) = t.
$$
This, finally, tells us that 
$$
y_2 (t) = v(t) y_1(t) = t e^{rt}. 
$$
#### Small example
Consider
$$
y'' + y' + \frac{1}{4} y = 0
$$
This has characteristic equation
$$
r^2 +  r + \frac{1}{4} = \left(r+\frac{1}{2}\right)^2 = 0.
$$
And the general solution is
$$
y = c_1 e^{-t/4}  + c_2 t e^{-t/4}.
$$
