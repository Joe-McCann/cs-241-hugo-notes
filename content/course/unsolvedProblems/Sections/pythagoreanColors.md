---
title: Coloring Pythagorean Triples
linktitle: Coloring Pythagorean Triples
toc: true
type: book
date: 2023-01-16
draft: false
tags:
    - combinitorics
    - number theory
    - algorithms
    - ramsey theory

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5001
---

## Problem Description

The Pythagorean Triples Coloring Problem[^1] takes in the set of all integers from $1\leq k\leq n$ for some value of $n$ and asks the question, is it possible to assign a color to every value in our set, such that no set of pythagorean triples is all the same color?

For example, the set $\\{1,2,3,4,5\\}$ can be split into coloring all of $\\{1,2,3\\}$ being red, and $\\{4,5\\}$ being blue. This will make it so that no Pythagorean triple is all contained in a single set. Not every number is part of a Pythagorean Triple (for example $1,2$) so those numbers can actually be ignored for this problem as they can go into any set.

There was this question before

> **Boolean Pythagorean Triples**: For a set of integers $n\mathbb{Z}=\\{1,2,3,\ldots, n\\}$, does there exist two sets $A,B$ with $A\cup B=n\mathbb{Z}$ and $A\cap B=\varnothing$, such that for every Pythagorean Triple $a^2+b^2=c^2$ where $a,b,c\in n\mathbb{Z}$ all of $a,b,c$ at least $1$ of $a,b,c$ is in each of $A,B$?

It actually turns out that the answer to this problem is **no**. In fact this was proven in $2016$ that if $n\geq 7825$ then there does not exist a solution. However, the answer effectively was a brute force[^2] that checked a ridiculous amount of cases. In fact, the computation took $2$ days and spent $4$ years worth of computation power combined across all the CPUs.

This leads us to the following two open questions

### Current Work

> **Question**: Does there exist an algorithm that can verify more quickly that there does not exist a solution for $n=7825$?

> **Question**: Does there exist some amount of colors $k>2$ such that you can in fact color all Pythagorean triples for any value of $n$?[^3] 

[^1]: [Wikipedia Link](https://en.wikipedia.org/wiki/Boolean_Pythagorean_triples_problem)
[^2]: Not really a brute force but it was an inefficient algorithm
[^3]: I wonder if you can apply theorem of planar graphs to this
