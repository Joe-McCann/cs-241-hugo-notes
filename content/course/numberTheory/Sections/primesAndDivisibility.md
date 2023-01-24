---
title: Primes and Divisibility
linktitle: Primes and Divisibility
toc: true
type: book
date: 2023-01-16
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

> Definition: Let $a,b\in\mathbb{Z}$. $a$ is __divisible__ by $b$ if we can find a value of $k\in\mathbb{Z}$ such that $a=k\cdot b$. This is notated as $b|a$ or "b __divides__ a". If $b$ divides $a$ then $b$ is a __factor__ of $a$.

Clearly every number is a factor of itself, and has $1$ as a factor

> Theorem: Let $a\in\mathbb{Z}$. $1|a$ and $a|a$
<details>
<summary>Proof</summary>
Since every number multiplied by $1$ is itself, we can know that $a=1a$ which by definition means that both $1,a$ are factors of $a$.
</br>
<b>Q.E.D.</b>
</details>
