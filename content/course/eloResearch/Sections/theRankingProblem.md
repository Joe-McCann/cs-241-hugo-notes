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

In many applications we will find that it is necessary to take a discrete, countable set of items $A$ and provide some ranked ordering of them. Personally, I have done this with movies I have watched[^1] as a way to know what the "best" and "worst" movies I have watched are. For a more practical application, it is important to know who the best candidates are given some open position and job applicants. We would like to be able to define some relation $\leq$ on $A$ so that if $a_i,a_j\in A$ then we could say $a_i\leq a_j$ or $a_j\leq a_i$. Note this relation may not necessarily be transitive. 

Of course, how do we even say some movie is better than another movie, how could we possibly define such a relationship? Even worse, how could we do math on movies 🤔?! We will define the following helpful tool that will allow us to actually do math

> **Definition**: Let $A$ be a set of items with relation $\leq$. We define a **skill function** $S:A\rightarrow\mathbb{R}$ to be a function where for $a_1,a_2\in T$ then $S(a_1)\leq S(a_2)$ if and only if $a_1 \leq a_2$. We say that $S(a_1)=\rho_1$ is the **skill** of $t_1$, with 
$$
\vec{\rho}=\left(S(a_1), S(a_2), S(a_3), \ldots\right)^T
$$

As noted above, $\leq$ may not be transitive, so an exact skill function may not exist, and thus our function $S$ will be an approximation of the true $\leq$ relation.

This has apparent parallels to [utility functions](https://en.wikipedia.org/wiki/Utility)[^2] in Game Theory and Economics.

In this definition, all that truly matters is the ordering, which means that there are infinite possible skill functions for a given ordering of items, however depending on the model or algorithm under consideration, different skill functions will be selected.

Perhaps it should have been asked earlier, but is this desired ranking even truly feasible or practical for modeling real world situations? Consider ranking sports teams: it is very possible that Team A always beats Team B, who always beats Team C, who always beats Team A. In this scenario of a cycle, who is the best? It could be argued that maybe they should all be equal, but as more items get added into the mix this becomes an impossible task. In the real world, transitivity of ordering is not so neat, and thus a single rank-order list often obscures some bizarre (and thus fun) dynamics.

Not just that, but often we don't have such a clear cut direction of $a_i\leq a_j$ or $a_j\leq a_i$. In sports, for example, "better" teams lose to "worse" teams all the time, and thus we can see that given any two items $a_1, a_2$, there is a random chance with probability $p$ that $a_1\leq a_2$ and $1-p$ that $a_2\leq a_1$. As such, we find it useful to define a matrix that can give us this narrower insight.

Of course, this makes the following assumptions
1. That all games have a binary outcome of win-loss
2. All teams are "fixed skill" in that win probabilities do not change over time

With these assumptions we can define the following matrix

> **Definition**: Let $A$ be a set of items and $P$ be the probability matrix defined such that for $t_i, t_j\in T$ $\Pr(t_i\leq t_j)= P_{ij}$.

This idea of a pairwise probability measure appears to be similar to the [Bradley-Terry Model](https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model)[^3]. However, in the Bradley-Terry model, there are $n$ values for the $n$ items, which is distinct from our formulation in which we have $\mathcal{O}(n^2)$ parameters.

[^1]: As of right now, my top 5 movies are Memento, Parasite, Saving Private Ryan, The Dark Knight, and Star Wars: Revenge of the Sith.
[^2]: https://en.wikipedia.org/wiki/Utility
[^3]: https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model