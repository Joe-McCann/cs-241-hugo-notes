---
title: Collatz Conjecture 💀
linktitle: Collatz Conjecture ☠️
toc: true
type: book
date: 2023-09-27
draft: false
tags:
    - number theory

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5004
---

## Problem Description

This problem has claimed the sanity of more mathematicians than you can possibly imagine. It is not for the faint of heart.

It might look simple, but its not.

> **Collatz Conjecture**: Start with some positive integer value of $x_0$ and apply the following process: if $x_i$ is even, then divide by $2$, if $x_i$ is odd then multiply by $3$ and add $1$. Does every initial value of $x_0$ eventually reach $1$?

This is largely believed to be true, and has been verified for numbers up to $2^{68}$, but is still unknown 🤷

## Collatz Cycles

In this page, I will **not** attempt to prove the Collatz Conjecture (I'm not a freak), but rather just do some cool things related to cycles one can in theory find.

When it comes to the Collatz Conjecture, there are two possible ways that it could be false: 
1. There is some sequence that grows infinitely
2. There is some cycle that never hits $1$

On this page we will observe what such properties Collatz Cycles would have to have. A lot[^1] of research has looked into these cycles, but I wonder if I can do it in a way that, if not groundbreaking, is at least easier to understand. Consider running a Collatz process starting with some integer $x$. Notice that at any point if we run the odd step, $3x+1$ creates an even number, meaning the next step must be an even one.

As such, if we wanted to observe a cycle, it is defined by how many factors of $2$ you divide by at every iteration. We will imagine that the "entrypoint" to our cycle is some number $x$. Let's define how we will represent the cycle that we have

> **Definition**: An $n$-cycle is a sequence of values $(x_1, x_2, \ldots, x_n)$ such that
$$
x_{i+1} = \frac{3x_i+1}{2^{k_i}}
$$
for $i<n$ and 
$$
x_{1} = \frac{3x_n+1}{2^{k_n}}.
$$
We say that the *process* of this cycle is the $n$-tuple $(k_1, k_2, \ldots, k_n)$ which represents what powers of $2$ we divide by at every iteration.

Notice that we did not actually specify that values of $x$ must be integers. This is because we are going to be asking the question in reverse than normal: rather than saying "what value of $x$ would have a cycle?", we are instead going to say "if we want a cycle following these steps, what would our $x$ values need to be?". 

> **Definition**: An $n$-cycle in which all $x_j$ are positive odd integers is a Collatz Cycle.

We specify that they must be odd integers to cover the case in which there are still factors of $2$ remaining that could have been removed. Note that this would form a counter example to the Collatz Conjecture.

Obviously, this shows that by extension if any of the $x_j$ are not integers, then it is not a valid Collatz Cycle.

### Connection to Existing Literature

As mentioned prior, there have been existing discussions regarding Collatz cycles, however they have considered cycles in a different form than as being described here. To rip from Wikipedia, which summarizes the history fairly effectively

> Steiner (1977) proved that there is no 1-cycle other than the trivial (1; 2). Simons (2005) used Steiner's method to prove that there is no 2-cycle. Simons and de Weger (2005)[^2] extended this proof up to 68-cycles; there is no k-cycle up to k = 68. Hercher extended the method further and proved that there exists no k-cycle with k $\leq$ 91[^3].

Of course, a cycle is a cycle, the difference between what my methods entail and the existing literature is in the notation and the cycles that they look at. A *Steiner Cycle* or *S-Cycle* is a cycle defined by the number of odd steps (followed by a single division by $2$) and then a number of even steps. Comparatively, in our formulation we only concern ourselves with the powers of $2$ divided by after each $3x+1$.

For example, what in our notation we would define as a $3$-cycle of $(1,1,3)$ they would define as the $2$-cycle that has $3$ odd steps $\frac{3x+1}{2}$ followed by $2$ even steps $\frac{x}{2}$. Of course, the cycles themselves are the same, just the difference is in the form of notation.

### Example of a $3$-Cycle

> **Example**: Find the starting point of the $3$-cycle with process $(3, 5, 1)$.

Woah Joe! Aren't you asking us to disprove the Collatz Conjecture in this way? No and [DON'T ASK ME AGAIN](https://www.youtube.com/shorts/hbvqOgeT1tE)[^4]. Since we do not need the values to be integers, what we find may not necessarily be a Collatz Cycle.

It turns out that using some incredibly straightforward algebra we can see this cycle has that
$$
\frac{3\frac{3\frac{3x+1}{2^3}+1}{2^5}+1}{2} = x
$$
which after simplifying and solving for $x$ we get that
$$
\begin{align}
(2^{3+5+1}-3^3)x &= 2^{3+5} + 3\cdot 2^{3} + 3^2 \\\\
485x &= 256 + 24 + 9 \\\\
x &= \frac{289}{485}.
\end{align}
$$

Now actually plugging this in we see that

$$
\begin{align}
3\left(\frac{289}{485}\right) + 1 &\rightarrow \frac{1352}{485} \\\\
\frac{1352}{485}\cdot \frac{1}{2^3} &\rightarrow \frac{169}{485} \\\\
3\left(\frac{169}{485}\right) + 1 &\rightarrow \frac{992}{485} \\\\
\frac{992}{485}\cdot \frac{1}{2^5} &\rightarrow \frac{31}{485} \\\\
3\left(\frac{31}{485}\right) + 1 &\rightarrow \frac{578}{485} \\\\
\frac{578}{485}\cdot \frac{1}{2} &\rightarrow \frac{289}{485}.
\end{align}
$$

This is super cool! We can expand this expression to allow us to find the starting point of any $n$-cycle process $(k_1, k_2, \ldots, k_n)$
$$
x = \frac{\sum_{i=0}^{n-1} 3^{n-1-i}2^{\sum_{j=1}^i k_j}}{2^{\sum_{i=1}^n k_i}-3^n}.
$$

Not the prettiest formula, but still an interesting one for sure, as this shows us that for every $n$-cycle process there is **exactly** one $n$-cycle that follows it, with one caveat.

If we state that $x_1$ is an *entrypoint* of our $n$-cycle, it becomes important to mention that our $n$-cycle is unique up to cyclic rotations, as there is no reason that $x_1$ specifically needed to be our entrypoint. In fact $(3,5,1), (5,1,3), (1,3,5)$ all will form the same $n$-cycle.

---

## A Simple Proof That There are No Collatz $2$-Cycles

Let us consider a $2$-cycle $(k_1, k_2), (k_2, k_1)$, which is the same cycle just different entrypoints. We will denote $x_1$ to be the entrypoint of $(k_1, k_2)$ and $x_2$ the entrypoint of $(k_2, k_1)$. 

We can use our entrypoint formula to see that
$$
\begin{align}
\left(2^{k_1+k_2} - 3^2\right)x_1 &= 2^{k_1} + 3 \\\\
\left(2^{k_1+k_2} - 3^2\right)x_2 &= 2^{k_2} + 3
\end{align}
$$

Let $d = 2^{k_1+k_2} - 3^2$ for notational simplicity.

This means that in order for $x_1, x_2$ to be integral, we need the following to be true
$$
\begin{align}
d &| 2^{k_1} + 3 \\\\
d &| 2^{k_2} + 3.
\end{align}
$$

Without loss of generality, suppose that $k_1 \leq k_2$. We know that
$$
d | (2^{k_2} + 3) - (2^{k_1} + 3) = 2^{k_1}(2^{k_2-k_1}-1).
$$

We can also say that $\gcd(d, 2)=1$ since $d$ is odd, so finally we have that 
$$
d | 2^{k_2-k_1} - 1.
$$

This actually gives us the trivial solution when $k_1=k_2$ which corresponds to the trivial cycle of $(2)$ repeated twice. This is the classic $1\rightarrow 4\rightarrow 2\rightarrow 1$ cycle. We will now look for other solutions

Since $d$ is a divisor, we know that it must be less than or equal to what it divides, so
$$
\begin{align}
2^{k_1+k_2} - 9 &\leq 2^{k_2 - k_1} - 1 \\\\
\implies 2^{k_1+k_2} - 2^{-k_1+k_2} &\leq 8 \\\\
\implies 2^{2k_1} - 1 \leq 2^{k_1+k_2} - 2^{-k_1+k_2} &\leq 8 \\\\
\implies k_1 &\leq \frac{\log_2(9)}{2} < 2.
\end{align}
$$

Since our values $k$ must be positive, this gives $k_1=1$ as the only possible value of $k_1$. We can now use this to bound $k_2$
$$
\begin{align}
2^{k_2+1} - 9 &\leq 2^{k_2 - 1} - 1 \\\\
\implies 2^{k_2}(2+\frac{1}{2}) &\leq 8 \\\\
\implies 2^{k_2} &\leq \frac{16}{5} \\\\
\implies k_2 &\leq \log_2\left(\frac{16}{5}\right)< 2.
\end{align}
$$

Meaning that our only possible cycle is $(1,1)$, which is a $1$-cycle with entrypoint $-\frac{2}{5}$, and thus not a valid Collatz $2$-cycle. QED.

---

## Trivial Bounds

Given some $n$-cycle process $(k_1, k_2,\ldots, k_n)$ producing entrypoint
$$
x_1 = \frac{3^{n-1}+3^{n-2}2^{k_1}+3^{n-3}2^{k_1+k_2}+\ldots+2^{k_1+k_2+\ldots+k_{n-1}}}{2^{k_1+k_2+\ldots+k_n}-3^n}
$$
we can immediately see the first trivial bound that
$$
\begin{align}
2^{k_1+k_2+\ldots+k_n}-3^n &> 0 \\\\
2^{k_1+k_2+\ldots+k_n} &> 3^n \\\\
k_1+k_2+\ldots+k_n &> n\cdot\log_2(3)
\end{align}
$$

> **Trivial Upper Bound UB1**: Given some $n$-cycle process $(k_1, k_2,\ldots, k_n)$, then in order for it to produce a Collatz Cycle then it is necessary that
$$
k_1+k_2+\ldots+k_n > n\cdot\log_2(3)
$$

This is not particularly surprising. The odd steps increase more than the even steps decrease, so we need to have more even steps in order to "balance out" and return back to our starting point.

[^1]: https://cs.uwaterloo.ca/journals/JIS/VOL26/Hercher/hercher5.pdf
[^2]: https://web.archive.org/web/20220318094356/http://deweger.xs4all.nl/papers/%5B35%5DSidW-3n+1-ActaArith%5B2005%5D.pdf
[^3]: https://cs.uwaterloo.ca/journals/JIS/VOL26/Hercher/hercher5.pdf
[^4]: Seriously though, who doesn't know Pikachu? 