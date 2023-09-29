---
title: Factorial Products ❗
linktitle: Factorial Products ❗
toc: true
type: book
date: 2023-01-16
draft: false
tags:
    - number theory
    - algorithms
    - factorial

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5002
---

## Problem Description

A very interesting property of $10!$ is that
$$
10!=6!\cdot 7!
$$

Can we find other examples of factorials that are products of other factorials like this? Clearly there are "trivial" answer such as
$$
24!=4!\cdot 23!
$$
where we say an example $n!=a!b!$ is "trivial" if $n=a!$ and $b=n-1$. As such we have the interesting problem of

> **Factorial Product[^3]**: Does there exist any other non-trivial factorial products $n!=a!b!$ other than $n=10,a=6,b=7$?

It is known that there are no nontrivial identities for $n\leq 18,160$[^1]. It has also been shown that if the *abc Conjecture* is true, then there are only finitely many non-trivial examples[^2].

[^1]: Information on [Wolfram Math](https://mathworld.wolfram.com/FactorialProducts.html)
[^2]: [Luca, F. (2007, November). On factorials which are products of factorials. In Mathematical Proceedings of the Cambridge Philosophical Society (Vol. 143, No. 3, pp. 533-542). Cambridge University Press.](https://www.cambridge.org/core/journals/mathematical-proceedings-of-the-cambridge-philosophical-society/article/abs/on-factorials-which-are-products-of-factorials/5763BE00D554BFC9A1D676E338FDC7E1)
[^3]: *Guy, R. (2004). Unsolved problems in number theory (Vol. 1). Springer Science & Business Media.*