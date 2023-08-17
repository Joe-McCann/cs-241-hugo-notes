---
title: Primes and Divisibility
linktitle: Primes and Divisibility
toc: true
type: book
date: 2023-08-15
draft: false
tags:
    - cs241
    - number theory
    - primes
    - integers
    - divisibility

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 205
---

## Integers Made up of Small Pieces

Think back to elementary school, a time when things were simple and you had recess. Despite the most difficult math being times tables, we still said math classes were hard 💀 oh how times have changed.

When learning about division, sometimes when you divided two numbers it wouldn't go in cleanly and there would be a *remainder* left over. For example, dividing $7$ by $3$ nets a remainder of $1$, and we would say that this meant $7$ was not *divisible* by $3$. But now we are big kids and the days of loose math terminology is over, what does it really mean for a number to be divisible by another?

> **Definition**: Let $a,b\in\mathbb{Z}$. $a$ is **divisible** by $b$ if we can find a value of $k\in\mathbb{Z}$ such that $a=k\cdot b$. This is notated as $b|a$ or "b **divides** a". If $b$ divides $a$ then $b$ is a **factor** of $a$.

Clearly every number is a factor of itself, and has $1$ as a factor, which we can prove.

> **Theorem**: Let $a\in\mathbb{Z}$. $1|a$ and $a|a$
{{% callout info %}}
<details>
<summary>Proof</summary>
Since every number multiplied by $1$ is itself, we can know that $a=1a$ which by definition means that both $1,a$ are factors of $a$.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

However most numbers have other factors too, for example $2|4$, $9|45$, or $25|100$. As such, numbers that *only* have $1$ and itself as factors are special, and we call them *prime*.

> **Definition**: An integer $p>1$ such that $p$ is only divisible by $1$ and $p$ is a **prime** number.

You could also rephrase this as: $p>1$ is prime iff for all $x>0$, if $x|p$, then $x=1$ or $x=p$. 

Note the requirement $p>1$, which restricts $1$ to not be a prime number. You might be wondering *"$1$ feels like it should be a prime number so why isn't it?"* The answer is quite bland in that, if $1$ was a prime number it would make so many theorems way more annoying to write out and deal with, and including $1$ as a prime number adds pretty much not benefit whatsoever. As such we just say $1$ is not prime out of convenience lmao. 

The sequence of prime numbers is an interesting one
$$
2,3,5,7,11,13,17,\ldots
$$
as there is seemingly no pattern at all. Many, many, mathematicians have spent many hours trying to figure out ways to express the prime numbers in a nice clear formula or expression, but none exists (and probably won't ever). We will talk about primes more later, but lets discuss a bit more on divisibility.

---

### Properties of Divisibility

Lets look at the relation[^1] $b|a$ when $a,b\in\mathbb{Z}$. We can begin with properties of relations

> **Theorem**: For all $a\in\mathbb{Z}$, $a|a$. This means divisibility is reflexive

This is actually already proven as before, so we don't need to do it again.

> **Theorem**: For all $a,b,c\in\mathbb{Z}$, if $a|b$ and $b|c$, then $a|c$. This means divisibility is transitive.

[^1]: You thought we were done with relations hahaha.
