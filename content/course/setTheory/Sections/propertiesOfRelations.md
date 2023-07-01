---
title: Properties of Relations
linktitle: Properties of Relations
toc: true
type: book
date: 2023-06-29
draft: false
tags:
    - cs241
    - sets
    - relations
    - functions

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 265
---

## Always More Terminology 📖

Now that we have relations, we can define some properties about them and see how our examples from the previous page compare. In all of our definitions, we will use $\*$ to represent some generic placeholder symbol, with $R(\*)\subseteq A\times A$ being our relation set.

### Reflexivity

Lets start with a motivating example, consider standard equality $=$ and any item $x\in\mathbb{R}$. Is $x=x$? Yes, by definition any value is always equal to itself! However, for some other relations like $<$ we cannot have $x<x$. Relations where every item is always related to itself are called *reflexive*.

> **Definition**: A relation $\*$ is **reflexive** if and only if for every $a\in A$, $a\*a$. In other words, every item is related to itself.

Lets go through our examples and determine if they are reflexive.

> **Example**: Is $a\equiv a \mod n$?

{{% callout info %}}
<details>
<summary>Answer</summary>
Yes, because the remainder of $a$ divided by $n$ will be the same on both sides of the equality
</details>
{{% /callout %}}

> **Example**: Is $x\leq x$ for all $x\in\mathbb{R}$?

{{% callout info %}}
<details>
<summary>Answer</summary>
Yes, because $x=x$ so $x\leq x$ by definition.
</details>
{{% /callout %}}

> **Example**: Is $p$.isParent$(p)$ for every person?

{{% callout info %}}
<details>
<summary>Answer</summary>
No, its not possible to be your own parent, so isParent is not reflexive.
</details>
{{% /callout %}}

### Symmetry

When we think about standard equality, notice that the order in which we perform our relation doesn't actually matter. If I tell you that $x=y$ is true, you automatically know that $y=x$ is also true. This property isn't always the case for a general relation (try thinking of one that isn't!), and we will call relations that are *symmetric*.

> **Definition**: A relation $\*$ is **symmetric** if and only if for every $a,b\in A$, $a\*b\implies b\*a$. In other words, the order in which you related items doesn't effect the truth.

Lets go through our examples and determine if they are symmetric.

> **Example**: Let $a\equiv b\mod n$. Does this imply that $b\equiv a\mod n$? 

{{% callout info %}}
<details>
<summary>Answer</summary>
Yes, because the changing the order of $a,b$ does not change the remainders when divided by $n$
</details>
{{% /callout %}}

> **Example**: Let $x\leq y$, does this mean $y\leq x$ **for all values** $x,y$

{{% callout info %}}
<details>
<summary>Answer</summary>
If $x=y$ you can switch their order, however if $x\neq y$ then you can't. For example $2\leq 3$ but $3\not\leq 2$. Therefore, $\leq$ is not symmetric
</details>
{{% /callout %}}

> **Example**: If $p$.isParent$(q)$, does that mean that $q$.isParent$(p)$?

{{% callout info %}}
<details>
<summary>Answer</summary>
No, your child cannot also be your parent in any case, this relation is not symmetric.
</details>
{{% /callout %}}

### Transitivity

*Transitivity* is a property that people use (and misuse) in online arguments all the time. Normally we know that if $x=y$ and $y=z$ then we can clearly chain them together to say that $x=z$, but not every relation has this property.

> **Definition**: A relation $\*$ is **transitive** if and only if for every $a,b,c\in A$, $a\*b,b\*c\implies a\*c$. In other words, you can chain relations together.

### Anti-Symmetry

### Connexity

## Classes of Relations