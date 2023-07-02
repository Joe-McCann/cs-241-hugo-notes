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

> **Definition**: A relation $\*$ is **transitive** if and only if for every $a,b,c\in A$, then $a\*b$ and ,$b\*c\implies a\*c$. In other words, you can chain relations together.

> **Example**: Let $a\equiv b\mod n$ and $b \equiv c \mod n$. Does this imply that $a\equiv c\mod n$? 

{{% callout info %}}
<details>
<summary>Answer</summary>
Yes, because all three remainders are equal to each other
</details>
{{% /callout %}}

> **Example**: Let $x\leq y$ and $y\leq z$, does this mean $x\leq z$

{{% callout info %}}
<details>
<summary>Answer</summary>
Yes, $x\leq y\leq z$ means $x\leq z$
</details>
{{% /callout %}}

> **Example**: If $p$.isParent$(q)$ and $q$.isParent$(r)$, does that mean that $p$.isParent$(r)$?

{{% callout info %}}
<details>
<summary>Answer</summary>
No, we only need one counter example and my grandfather is not one of my parents. That'd be fucked y'all
</details>
{{% /callout %}}

### Anti-Symmetry

The term *anti-symmetry* might make you think *"Oh this probably means not symmetric"*, which certainly would make a lot of sense. Sadly you would be wrong because mathematicians are bad at naming things (I would call this *conditional symmetry* but fuck me I guess). Remember above in our symmetry section where we saw that sometimes we had $x\leq y$ **and** $y\leq x$, but only if $x=y$? Thats what anti-symmetry means, the only time you are allowed to switch the order is if $x=y$!

> **Definition**: A relation $\*$ is **anti-symmetric** if and only if for every $a,b\in A$, then $a\*b$ and, $b\*a\implies a=b$. In other words, the relation is only symmetric if $a=b$.

For the sake of not showing confusing examples, I am going to show two. isParent is a bit of a confusing edge case, but an interesting problem to think about.

> **Example**: Let $x\leq y$ and $y\leq x$, does this mean $x=y$

{{% callout info %}}
<details>
<summary>Answer</summary>
Yes, that is the only way we can have both conditions be satisfied, so $\leq$ is anti-symmetric.
</details>
{{% /callout %}}

> **Example**: Let $a\equiv b\mod n$ and $b \equiv a \mod n$. Does this imply that $a=b$? 

{{% callout info %}}
<details>
<summary>Answer</summary>
No, consider $0,n$. Both of these have remainder $0$ divided by $n$, so $0\equiv n\mod n$ and $n\equiv 0\mod n$ but $n\neq 0$.
</details>
{{% /callout %}}

### Connexity

Our final property is one that is very specifically relevant in certain situations. Notice that given two items, its very often the case that they are just not related at all. For example $2\neq 3$. But for some relations, its also totally possible that we can arrange any two items in a way such that we can always relate them.

> **Definition**: A relation $\*$ is **connex** if and only if for every $a,b\in A$, then $a\*b$ or, $b\*a$ (or both). In other words, you can always relate two items.

> **Example**: For all $x,y$ is $x\leq y$ or $y\leq x$

{{% callout info %}}
<details>
<summary>Answer</summary>
Yes, if the two numbers are equal then the order doesn't matter. If they are not then one is smaller and you can put that one first.
</details>
{{% /callout %}}

> **Example**: Given any $a,b$ is it guaranteed that $a\equiv b\mod n$ or $b\equiv a\mod n$?

{{% callout info %}}
<details>
<summary>Answer</summary>
No, changing the order will not change the remainders, so if they are not related to begin, then the two items are not related, therefore this is not connex.
</details>
{{% /callout %}}


---

## Classes of Relations

Now that we have these properties, we can define three types of relations.

> **Definition**: If a relation is reflexive, symmetric, and transitive, then it is an **equivilence relation**.

This is a very important type of relation and we will discuss it later on the next page. These relations do all the things that we would like when setting things equal to each other, so they make things equivilent in a way. In our examples equality $\mod n$ was an equivelence relation

> **Definition**: If a relation is reflexive, transitive, and anti-symmetric, then it is a **partial order relation**. If a partial order is also connex, then it is a **total order relation**.

In our examples, we had that $\leq$ was a total order, which is cool because these sorts of relations define a way of ordering your set, so to speak.

---

## Practice Problems

> **Example**: We said above that *anti-symmetric* does not mean *not symmetric*. Is it possible for a relation to be both symmetric and anti-symmetric?

> **Example**: Is isParent anti-symmetric?