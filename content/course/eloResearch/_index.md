---
# Course title, summary, and position.
title: Ranking Research
linktitle: Ranking Research
summary: Making League of Legends more toxic since 2026
weight: 30

# Page metadata.
date: "2023-09-01T00:00:00Z"
lastmod: "2023-09-01T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: book  # Do not modify.
tags: 
- unsolved
- elo

---

**William Joe McCann**

## Wreaking Elo Havoc

In this page I will write up notes dedicated to the problems that I have been looking at in the world of ranking algorithms and more specifically the ELO algorithm.

Despite having been around for a very long time, it is not entirely clear exactly how the dynamics of the ELO algorithm work in the theoretical sense, and this page will be dedicated some some interesting problems that come from it.

## A Table of Notation

For each section, we will introduce new notation for that specific section. For in this table, we will define notation that will be used across all pages within this entire category of ranking research. We assume background knowledge of set theory, calculus, probability, and statistics.

### General Rankings Notation

1. $n$ is the number of items to be ranked
2. $A=\left\\{a_1, a_2, \ldots, a_n\right\\}$ is the set of items. For brevity we will often have notate this as the set $\left\\{1,2,3,\ldots, n\right\\}$.
3. Let $A$ be equipped with relation $\leq$ to which we say that if $a_i< a_j$ then $a_j$ is "more skilled" than $a_i$, and if $a_i \leq a_j \land a_j \leq a_i$ then they are "equally skilled". Note this may *not* necessarily be an ordering, as we may have cycles.
4. $S: A\rightarrow \mathbb{R}$ is a "skill function" that has the property that $S(a_i) \leq S(a_j)$ iff $a_i \leq a_j$. Note, different algorithms will induce different functions $S$, which will be the object of the research in this section to understand these different functions and how well they model $\leq$. In the event that $\leq$ is non-transitive, we will be interested in how well it approximates $\leq$
5. Should a function $S$ exist, let $\rho_i=S(a_i)$, and $\vec{\rho}=\left(S(a_1), S(a_2), \ldots, S(a_n)\right)^T$. We will say these values are the "true skill" values of out items[^2].
6. $\mathbb{M}=(m_t)_{t\in\mathbb{N}}$ is our sequence of "matches" indexed by integer timestamp $t$, where $m_i\in M\subset A\times A$.
7. $\Omega$ is the set of possible results for a match. For example, a game such as "Rock-Paper-Scissors" must always end in a win-loss state[^1] so $\Omega_{\text{RPS}}=\\{(0,1),(1,0)\\}$ with $0$ being a loss, and $1$ being a victory. A game like soccer though would have $\Omega=\mathbb{N}^2$[^3].
8. $\mathscr{M}:M\rightarrow \Omega$ is the a function that takes a match and provides the result of that match[^4] from a realized history perspective.
9. $K_{ij}(\Omega)$ is a probability distribution that provides the probability of all outcomes given match $(a_i, a_j)$
10. $P$ is the pairwise *true* probability matrix in which $P_{ij}$ represents that player $a_i$ wins over $a_j$ in the case of win-loss games
11. $\sigma:\mathbb{R}\rightarrow\mathbb{R}$ is a general sigmoid function. Note some algorithms may introduce additional properties on top of their selected sigmoid.

[^1]: Assuming we live in the finite world where it is $0$ probability that we have an infinite sequence of ties
[^2]: There are much discussions to be had about whether such "true" skills even exist, if they are unique, if different projections or algorithms can result in different $\vec{\rho}$, but here we will just leave it as notation in the "just in case"
[^3]: Naturals start at $0$ you freaks
[^4]: We will not use this in practice, but I find it useful to have at least some theoretical framework behind what we do.
