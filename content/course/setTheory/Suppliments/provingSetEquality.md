---
title: Proving Set Equality
linktitle: How to Prove Two Sets Are Equal
toc: true
type: book
date: 2023-01-31
draft: false
tags:
    - cs241
    - set theory
    - set operations

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 310
---

## You Can Go Your Own Way 🎵 🎶

In this page I will explain one way of proving that two sets or set identities are equal. Obviously, there is never exclusively one way to do these things, so if you (for example) decide to learn algebraic identities for sets, then you are totally free to do so. This is just the way I learned it and I find it easy to understand.

### Worked Example

> **Theorem** (De Morgan's Laws): Let $A,B$ be sets. $$\overline{(A\cup B)}=\overline{A}\cap\overline{B}$$

The purpose of this section is to walk through proving this law, and then providing other examples that you can try. Remember that we said from the [theorems on set equality](/course/serTheory/sections/proofbycontradiction#no_largest_number_theorem) that two sets are equal if and only if they are subsets of each other, so we can start by doing that first.

In order to first show that $$\overline{(A\cup B)}\subseteq\overline{A}\cap\overline{B}$$, we want to show that if there is any item that is on the left hand side, that item must also be on the right hand side. Suppose we pick some item $x\in\overline{(A\cup B)}$.

We know by the definition of complement that this means that $x\not\in (A\cup B)$. As such, $x\not\in A$ **and** $x\not\in B,$ otherwise $x$ would be in the union. We can get from this that $x\in\overline{A}$ and $x\in\overline{B}$ which means that $$x\in\overline{A}\cap\overline{B}.$$ This proves the left side is a subset of the right side.

Now we have to show that $$\overline{A}\cap\overline{B}\subseteq\overline{(A\cup B)}.$$

Select some item $x\in\overline{A}\cap\overline{B}$, we know that $x\in\overline{A}$ and $x\in\overline{B}$. Hopefully you can see where this is going and that its the same as the previous way, just in reverse. I'm just going to write out the steps from here
$$
\begin{aligned}
& x\in\overline{A} \text{ and } x\in\overline{B} \\\\
\implies& x\not\in A \text{ and } x\not\in B \\\\
\implies& x\not\in (A\cup B) \\\\
\implies& x\in\overline{A\cup B} \\\\
\implies& \overline{A}\cap\overline{B}\subseteq\overline{(A\cup B)}.
\end{aligned}
$$

Since the two sets are subsets of each other, then we know that the two sets must be equal, **Q.E.D.**

This method (while bland), is a very straightforward and easy way to show sets are equal to each other.

## More Examples

> **Theorem**: $$\mathcal{P}(A\cap B) = \mathcal{P}(A)\cap\mathcal{P}(B)
