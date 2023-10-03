---
title: Robin's Inequality 🐦
linktitle: Robin's Inequality 🐦
toc: true
type: book
date: 2023-09-27
draft: false
tags:
    - number theory
    - prime numbers

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5005
---

## Problem Description

This problem is one that I have tinkered around with a lot, and work that I am actually quite proud of. Sadly this problem is an equivalent formulation as the Riemann Hypothesis, so I will likely never actually prove it, however theres some cool progress that I can make on manual computations.

Before we can actually formulate Robin's Inequality, we need to define ourselves a function $\sigma$ where $\sigma(n)$ is the *sum of divisors function*.

> **Definition**: Let $\sigma(n):\mathbb{N}\rightarrow\mathbb{N}$ be the sum of all positive integers $k \leq n$ such that $n$ is divisible by $k$

Lets give some examples of values of $\sigma(n)$. For example we can have that
$$
\begin{align}
\sigma(6) &= 1 + 2 + 3 + 6 = 12 \\\\
\sigma(7) &= 1 + 7 = 8\\\\
\sigma(24) &= 1 + 2 + 3 + 4 + 6 + 8 + 12 + 24 = 60
\end{align}
$$

Now we can provide the statement of the conjecture

> **Robin's Conjecture**: For all $n > 5040$ then
$$
\sigma(n) < e^\gamma n\log(\log(n))
$$
where $\gamma=.57721\ldots$ is the Euler-Mascroni constant. In this post we are just going to say "all values of" when actually talking about values of $n>5040$.

### Problem in Terms of Prime Products

An interesting thing to notice though is that if we know the prime decomposition of our number, then we actually can pretty easily compute the value of $\sigma(n)$ without having to go through the process of finding all factors. For example $24=2^3\cdot 3$ so
$$
\sigma(24)= 2^03^0 + 2^03^1 + 2^13^0 + 2^13^1 + 2^23^0 + 2^23^1 + 2^33^0 + 2^33^1= 60.
$$

In fact, if we say that $n=p_1^{k_1}p_2^{k_2}\ldots p_m^{k_m}$ with values $p_j$ being prime, we can actually get a closed form expression for $\sigma(n)$
$$
\sigma(n) = \prod_{j=1}^m \frac{p_j^{k_j+1}-1}{p_j-1}.
$$
We can get this from seeing that
$$
\sigma(24)=(2^3+2^2+2^1+2^0)(3^1+3^0)=\frac{2^4-1}{2-1}\cdot\frac{3^2-1}{3-1},
$$
and generalizing it from there.

For convenience sake, we will order the primes in our prime products so that $p_1 < p_2 < p_3 < \ldots < p_m$. This means that if we rearrange Robin's inequality to the form
$$
\frac{\sigma(n)}{e^\gamma n} < \log\left(\log(n)\right) 
$$
and plug in our prime decomposition form of $n$ we can find that
$$
e^{-\gamma}\prod_{j=1}^m \frac{p_j^{k_j+1}-1}{p^{k_j}(p_j-1)} < \log\left(\sum_{j=1}^m k_j\log(p_j) \right).
$$

This might seem like its making it more difficult, but its actually making it so that instead of working with values of $n$, we can instead work with various prime products! If we could show this is true for all prime products that evaluate $>5040$, then that would prove the conjecture. It turns out that a lot of cases have actually been proven.

---

## Other People's Progress

I am (obviously) not the first person to be looking into the mechanics of this inequality, and some types of numbers have already been proven to be true. From prior research[^1], we know these lemma's are true assuming $n > 5040$

> **Squarefree Lemma**: If $n=p_1p_2p_3\ldots p_m$ such that no power of a prime of $n$ is greater than $1$, then Robin's Inequality is true for $n$.

> **Squarefull Lemma**: If $n=p_1^{k_1}p_2^{k_2}\ldots p_m^{k_m}$ and every $k_j\geq 2$ then Robin's Inequality is true for $n$

> **Fivefree Lemma**: If $n=p_1^{k_1}p_2^{k_2}\ldots p_m^{k_m}$ and every $k_j < 5$ then Robin's Inequality is true for $n$

> **Sum of Squares Lemma**: If $n=p_1^{k_1}p_2^{k_2}\ldots p_m^{k_m}$ and every $p_j$ that is equivalent to $3\mod 4$ has an even $k_j$, then Robin's Inequality is true for $n$. This is equivalent to $n$ being the sum of two squared numbers.

> **20-Free Lemma**: If $n=p_1^{k_1}p_2^{k_2}\ldots p_m^{k_m}$ and every $k_j < 20$ then Robin's Inequality is true for $n$[^2]

> **Odd Lemma**: If $n$ is odd then Robin's Inequaility is true for $n$.

There are also now insane bounds on how large a counter example must be, which I won't write out here. 

---

## My Progress

This problem has been super interesting to me for a while, so I was able to prove a couple of cool and interesting things about it. 

> **Large Prime Lemma**: Let $n=p_1^{k_1}p_2^{k_2}\ldots p_m^{k_m}$ be a number that satisfies Robin's Inequality, then if $N=P_1^{k_1}P_2^{k_2}\ldots P_m^{k_m}$ with every $p_j\leq P_j$, then $N$ also satisfies Robin's inequality.

Funny story is that I posted this theorem to arXiv in 2020 and then posted it to Reddit when it was approved. I was told at the time that such theorem was obvious and that I shouldn't be sad that I made no contributions to mathematics.

This theorem was later part of a paper that was published to the American Mathematical Society in 2022[^3] 💀. To be fair they had other more interesting things in the paper, but doesn't mean that I don't feel vindicated lmfao.

The proof of this follows pretty easily from the fact that if $p < P$ then
$$
\frac{P^{k+1}-1}{P^k(P-1)} < \frac{p^{k+1}-1}{p^k(p-1)}
$$
but
$$
\log\left(k\log(p)\right) < \log\left(k\log(P)\right)
$$
so the inequality must always be preserved.

> **Sufficiently Large Powers Lemma**: Let $n=p_1^{k_1}p_2^{k_2}\ldots p_m^{k_m}$ be an odd number, then there exists a $K_n$ such that for all $k > K_n$, then $2^kn$ satisfies Robin's Inequality.

This one is similarly easy to show as
$$
\frac{2^{k+1}-1}{2^k} < 2
$$
but $\log\left(k\log(2)\right)$ is unbounded.

### Worst Case Values

Before we get into something that I haven't seen in another paper 🤞 I will introduce some new notation. This is usually a terrible idea but the expressions in this page are so long you'll thank me. Lets define for some $n=p_1^{k_1}p_2^{k_2}\ldots p_m^{k_m}$
$$
q_n = e^{-\gamma}\prod_{j=1}^m \frac{p_j^{k_j+1}-1}{p^{k_j}(p_j-1)}
$$
and
$$
\alpha_n = < \sum_{j=1}^m k_j\log(p_j).
$$
This means that Robin's Inequality is now simplified to be
$$
q_n < \log(\alpha_n).
$$
I believe when I initally was creating this notation I decided $q$ for "quotient" and $\alpha$ for no reason other than I can write it quickly. We will say also that
$$
\delta_n = \log(\alpha_n) - q_n
$$
which means that Robin's Inequality is to show that for all $n>5040$ that $\delta_n > 0$.

Suppose now that we have some value $n$ which is odd. We know by the **Odd Lemma** that $n$ satisfies, so $\delta_n > 0$. Let us now consider the new number $2^k\cdot n$ for variable values of $k\geq 1$. In order to find potential counter-examples, we want to find the value of $k$ that gives us the smallest $\delta$, so we will let
$$
\delta_n(k) = \log\left(k\log(2)+\alpha_n\right) - \frac{2^{k+1}-1}{2^k} q_n
$$
and we want to find
$$
\min_{k\in\mathbb{N}}\delta_n(k).
$$

We can do this via calculus by doing
$$
\frac{\partial}{\partial k}\delta_n(k) = 0
$$
which I will spare us the details of, but the solution is of the minimal $k$
$$
$k_{\text{min}} = -\frac{1}{\log(2)}\left(\alpha_n + W_{-1}\left(-\frac{1}{q_ne^{\alpha_n}}\right)\right)
$$

[^1]: Lagarias, J. C. (2002). An elementary problem equivalent to the Riemann hypothesis. The American Mathematical Monthly, 109(6), 534-543.

[^2]: Platt, D. J., & Morrill, T. (2021). Robin's inequality for 20-free integers. INTEGERS: Electronic Journal of Combinatorial Number Theory, 21, 28.

[^3]: Saouter, Y. (2022). New results for witnesses of Robin’s criterion. Mathematics of Computation, 91(334), 909-920.