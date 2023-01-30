---
title: Set Operations But Like, Harder
linktitle: Advanced Set Operations
toc: true
type: book
date: 2023-01-24
draft: false
tags:
    - cs241
    - set theory
    - cardinality
    - powerset
    - cartesian product

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 220
---

## Singular Set Operations

### Cardinality

Cardinality is super easy to understand (at least in the finite sense). Cardinality can be thought of a measure of how "big" a particular set is, and is just the count of how many items are inside the set.

> **Definition**: The **cardinality** of a set is the number of items inside that set, notated as $|A|$ for a set $A$.

> **Example**: Let $A=\\{1,2,3\\}$, what is $|A|$?
<details>
<summary>Answer</summary>
There are $3$ items in $A$, so $|A|=3$.
</details>

Note that this idea of "number of items", doesn't work when our sets are infinite. I mean sure we can say the size is $\infty$, but what does that truly mean? For now we will not discuss further, but we will revisit this when we talk about functions.

### The Powerset

Suppose I have some set $A=\\{1,2,3,4\\}$. We know that $\\{1,2\\}$ is a subset of $A$ because all the items are inside of $A$. We also know that $\\{1,3,4\\}$ is a subset of $A$ too. This is the same case of $\varnothing$. It seems like a set being a subset of $A$ is a property of a set that we can group into a new set! This is the idea of the powerset of a set!

> **Definition**: Given a set $A$, the **powerset** of $A$, $\mathcal{P}(A)$, is the set of all subsets of $A$, defined as $$\mathcal{P}(A)=\\{B | B\subseteq A\\}$$

For every set $A$, we know that both $A\subseteq A$ and $\varnothing\subseteq A$, so $\mathcal{P}(A)$ will always at least have those two items (unless $A=\varnothing$, then only $\varnothing$ is contained).

> **Example**: Let $A=\\{1,2,3\\}$, what is $\mathcal{P}(A)$?
<details>
<summary>Answer</summary>
$$\mathcal{P}(A)=\{\varnothing,\{1\},\{2\},\{3\},\{1,2\},\{1,3\},\{2,3\},\{1,2,3\}\}.$$
</details>

Its easy to see through examples that as you increase the size of $A$, the size of $\mathcal{P}(A)$ starts to get very, very big. It turns out that we can actually related these two sizes together with a very nice little theorem!

> **Theorem**: $|\mathcal{P}(A)|=2^{|A|}$
<details>
<summary>Proof</summary>
This theorem is simply proven by a counting argument. Consider how many subsets can possibly have of $A$, and that will be how many elements are inside the powerset. For any particular element $x\in A$, for every subset that element is either inside or not inside the subset. That means for each element, we have $2$ options per subset.
</br>
We can multiply $2\cdot 2\cdot\ldots\cdot 2$ for every element to represent the total number of subsets we can get. This will be done $|A|$ times, so we know the claim is true.
</br>
<b>Q.E.D.</b>
</details>

Since this grows exponentially, you can rest assured that I am not that much of a psychopath to ask you to manually write out the powerset of any set that has more than $4$ elements.

## Good Things Come in Pairs

Before we can define this next operator, we must first discuss a new type of mathematical object[^1].

[^1]: When writing this page I was exhausted on an airplane so sorry if its a little low effort.
