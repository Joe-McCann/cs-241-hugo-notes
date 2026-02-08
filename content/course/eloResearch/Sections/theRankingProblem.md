---
title: Ranking Items ⏫⏬
linktitle: Ranking Items ⏫⏬
toc: true
type: book
date: 2026-02-06
draft: false
tags:
    - unsolved
    - elo

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 6001
---

## Problem Description

In many applications we will find that it is necessary to take a discrete, countable set of items $T$ and provide some ranked ordering of them. Personally, I have done this with movies I have watched[^1] as a way to know what the "best" and "worst" movies I have watched are. For a more practical application, it is important to know who the best candidates are given some open position and job applicants. We would like to be able to define some total ordering relationship $R(\leq)\subset T\times T$ so that if $t_i,t_j\in T$ then we could say $t_i\leq t_j$ or $t_j\leq t_i$. 

Of course, how do we even say some movie is better than another movie, how could we possibly define such a relationship? Even worse, how could we do math on movies 🤔?! We will define the following helpful tool that will allow us to actually do math

> **Definition**: Let $T$ be a set of items with total order relation $R(\leq)$. We define a **skill function** $s:T\rightarrow\mathbb{R}$ to be a function where for $t_1,t_2\in T$ then $s(t_1)\leq s(t_2)$ if and only if $t_1 \leq t_2$. We say that $s(t_1)=\rho$ is the **skill** of $t_1$, with 
$$
\vec{\rho}=\left(S(t_1), S(t_2), S(t_3), \ldots\right)^T
$$

This has apparent parallels to [utility functions](https://en.wikipedia.org/wiki/Utility)[^2] in Game Theory and Economics.

In this definition, all that truly matters is the ordering, which means that there are infinite possible skill functions for a given ordering of items, however depending on the model or algorithm under consideration, different skill functions will be selected.

Perhaps it should have been asked earlier, but is this desired ranking even truly feasible or practical for modeling real world situations? Consider ranking sports teams: it is very possible that Team A always beats Team B, who always beats Team C, who always beats Team A. In this scenario of a cycle, who is the best? It could be argued that maybe they should all be equal, but as more items get added into the mix this becomes an impossible task. In the real world, transitivity of ordering is not so neat, and thus a single rank-order list often obscures some bizarre (and thus fun) dynamics.

Not just that, but often we don't have such a clear cut direction of $A\leq B$ or $B\leq A$. In sports, for example, "better" teams lose to "worse" teams all the time, and thus we can see that given any two items $t_1, t_2$, there is a random chance with probability $p$ that $t_1\leq t_2$ and $1-p$ that $t_2\leq t_1$. As such, we find it useful to define a matrix that can give us this narrower insight

> **Definition**: Let $T$ be a set of items and $P$ be the probability (or **odds matrix**) matrix defined such that for $t_i, t_j\in T$ $Pr(t_i\leq t_j)= P_{ij}$.

This idea of a pairwise probability measure appears to be similar to the [Bradley-Terry Model](https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model)[^3]. However, in the Bradley-Terry model, there are $n$ values for the $n$ items, which is distinct from our formulation in which we have $\mathcal{O}n^2$ parameters.

[^1]: As of right now, my top 5 movies are Memento, Parasite, Saving Private Ryan, The Dark Knight, and Star Wars: Revenge of the Sith.
[^2]: https://en.wikipedia.org/wiki/Utility
[^3]: https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model