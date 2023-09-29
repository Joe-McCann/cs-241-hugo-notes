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

---

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

---

## Good Things Come in Pairs

Before we can define this next operator, we must first discuss a new type of mathematical object[^1].

### Tuples

When we dealt with sets, we described a type of way to house objects that contained *unqiue* and *unordered* items. In computer science, this lends itself to the very useful `set` datastructure, however, this is not the most common type of datastructure that we encounter in the day-to-day of programming.

Quite often we want to be able to duplicate items, and sometimes the order matters! Suppose I wanted to collect all the first names of my students in alphabetical order. This would not be possible with a set, as we need to be able to alphabatize, rather than just check membership, and my students very often share first names! As such we need something that allows us to do this.

> **Definition**: A **tuple** is an ordered list of items, notated $T=(t_1,t_2,\ldots,t_k)$. A tuple with $1$ item is a **singleton**, a tuple with $2$ items is an **ordered pair**, and a tuple with $k>2$ items is a **k-tuple**.

This provides us with a way to mathematically represent the `lists` or `arrays` that we so frequently program with! Python also has a `tuple` datastructure which is just an immutable[^2] list.

There are theoretical ways to represent tuples as sets (so everything eventually collapses to a set), however, this is not necessary for our purposes.

---

### The Cartesian Product

Now that we have tuples, we want ways to generate lots of them. For example, say I want to create a set of all ordered pairs of integers, as a way of writing down a nice integer grid. I could in theory start by just writing out examples, or set builder notation, but this idea actually appears frequently enough where it's useful to give it some notation!

> **Definition**: Let $A,B$ be sets. The **Cartesian Product**, notated as $A\times B$ is given as the set of all ordered pairs who's first item is an element of $A$, and who's second is an element of $B$. In set builder notation $$A\times B=\\{(a,b)|a\in A, b\in B\\}.$$

So if we wanted to get that set of all ordered pairs of integers, we would just do
$$
\mathbb{Z}\times\mathbb{Z}=\\{(1,1),(0,0),(-1,2),(-2,0),\ldots\\}.
$$

From a notational standpoint, you will often see the set $A\times A$ be written as $A^2$. This is why when talking about the *cartesian coordinate plan* which is the set of all ordered pairs of real numbers, we write it as $\mathbb{R}^2$.

> **Example**: Let $A=\\{1,2\\}$, $B=\\{x,y\\}$, what is $A\times B$?
<details>
<summary>Answer</summary>
$$A\times B=\{(1,x),(1,y),(2,x),(2,y)\}.$$
</details>

Note that the order of the sets of the Cartesian product matter, so in general $A\times B\neq B\times A$[^3].

What do we do if we take the Cartesian Product of multiple sets? For example, what would be $A\times B\times C$? Would all the elements be these nested tuples like $((a,b),c)$? Rather than that, we actually have that for a $k$-Cartesian Product we get a $k$-tuple, so in our example,
$$
A\times B\times C = \\{(a,b,c)| a\in A, b\in B, c\in C\\}.
$$

In general
$$
A_1\times A_2\times\ldots\times A_k = \\{(a_1,a_2,\ldots,a_k)|a_1\in A_1, a_2\in A_2,\ldots,a_k\in A_k\\}.
$$

If take the cartesian product of the same set $k$ times, we notate as $A^k$

## Examples

> **Example**: Write out a deck of playing cards as a Cartesian Product of two sets, where each card is represented as a tuple of a value (number or face) and a suit. For example the $9$ of hearts would be ($9$, Hearts).

> **Theorem**: $$|A\times B|=|A|\cdot|B|$$

[^1]: When writing this page I was exhausted on an airplane so sorry if its a little low effort.
[^2]: Immutable means its cannot be changed once you create it.
[^3]: We say that the Cartesian product is not *commutative*
