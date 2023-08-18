---
title: Greatest Common Divisor and Division
linktitle: Division and the GCD
toc: true
type: book
date: 2023-08-15
draft: false
tags:
    - cs241
    - number theory
    - primes
    - integers
    - division
    - gcd
    - complexity

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 210
---

## Remainders and Division

Before we get to a topic that is incredibly important in number theory, lets talk about division of integers and the different pieces that you can get when you divide numbers up into integer parts. In our previous page, we talked all about divisibility, but what happens if we want to divide numbers that aren't divisible by each other? Like what is $\frac{25}{7}$? We don't have decimals in the integers? Does the universe just explode?

No, what we can do is what you did in grade school and split the division into a *quotient* and a *remainder*. As you might remember seeing it
$$
\frac{25}{7} = 3 + \frac{4}{7},
$$
where you would say that $3$ is the quotient and there is a remainder of $4$. Theres still no decimals in the integers, so lets multiply both sides by $7$ to get
$$
25 = 3\cdot 7 + 4.
$$

In general we want to be able to do this for any time we divide two numbers, is that possible? It turns out the answer is yes!

> **Euclidean Division Theorem**: Let $a,b$ be integers and $b\neq 0$. There exists a unique quotient $q$ and remainder $r$ such that
$$
a = qb + r
$$
and $0 \leq r < |b|$.

Damn, y'all probably are already sick of existence and uniqueness theorems huh? Lets first prove existence. We will be proving existence for $a,b > 0$, however this is sufficient to prove the claim as explained [here](https://en.wikipedia.org/wiki/Euclidean_division#Existence)
{{% callout info %}}
<details>
<summary>Existence Proof</summary>
We will show an algorithm that when repeated will provide us with a solution. First, begin by setting $q_0 = 0$ and $r_0=a$. By definition we know that
$$
a = q_0b + r_0.
$$
If $r_0 < b$ then we are done. If $r_0 \geq b$, then we will set $q_1 = q_0+1 = 1$ and $r_1 = r_0 - b$. You can plug in and see that
$$
a = q_1b + r_1.
$$
If $r_1 < b$ then we are done. If not then we repeat the process with $q_{n+1} = q_n + 1$, and $r_{n+1}=r_n-b$. With this method we will eventually find values of $q,r$ that satisfy $0\leq r < b$.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

Fun fact, this provides an algorithm for division! Just keep subtracting and keeping count until you get to a remainder $0\geq r < b$. This isn't an efficient algorithm as it runs in $\Theta\left(\frac{a}{b}\right)$, but hey its something. Now lets prove the second part
{{% callout info %}}
<details>
<summary>Uniqueness Proof</summary>
Let us assume that there is a value of $a,b$ for which the division has more than one value of $q,r$. So we have$$
\begin{align}
a &= q_1b + r_1 \\\\
a &= q_2b + r_2 
\end{align}$$
but $q_1\neq q_2$ or $r_1 \neq r_2$. Setting these equations equal to each other we get that
$$
q_1b + r_1 = q_2b + r_2 \implies b(q_1-q_2) = r_2-r_1.
$$
This means that $b|r_2-r_1$ and since both $0\leq r_1,r_2 < |b|$, this means that $r_2-r_1=0$. We then can plug this in to show 
$$
b(q_1-q_2)=0\implies q_1-q_2=0$$
since $b\neq 0$. As such we know that $q_1=q_2$ and $r_1=r_2$ contradicting our assumption and proving uniqueness.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

## The Greatest Common Divisor