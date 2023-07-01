---
title: Relations
linktitle: Relations
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
weight: 260
---

## Relating Things Together

Through the use of functions, we can define a lot of the things that we use in general, such as operations like $+$ and $\cdot$. As of right now though, we don't really have a good way of comparing things together. For example, how would we define a simple comparison like $x < y$? This is an important question as literally all the time we want to be relating and comparing things to each other; if you're a Java programmer you'll know about these as *comparators*.

One way that we can do it is to define a function $< : \mathbb{R}\rightarrow\\{0,1\\}$ and say that an output of $1$ represents **TRUE** and $0$ represents **FALSE**. This is totally fine, but a bit clunky and would require some of our properties we will show in the future would be hella fucking annoying to prove in this way. As such we will create a special type of set to do the work for us!

> **Definition**: A **relation** $R \subseteq A_1\times A_2\times\ldots\times A_n$ is a set of tuples $t$ for which we say that if $t\in R$ then the items in $t$ are *related*, and if $t\not\in R$ then the items in $t$ are *not related*. If $\*$ is an operation that relates two items together then $R(\*)\subseteq A\times B$ is a **binary relation** and $a\*b$ is **TRUE** if and only if $(a,b)\in R(\*)$.

In this sense, to define a way to related items together, you throw all the groups of items that are properly related in a set, and in order to check if the relation $a\*b$ is true (where $\*$ represents a general symbol) then just check if $(a,b)$ is a set member. For our cases we will be considering only binary relations, but this can be much more general.

---

### Examples of Relations

In this section we will provide some examples of relations that we will use on the next page in the discussion of properties of relations.

> **Example**: Let $R(=)=\\{(a,b)\in A\times A | \text{a,b are the same}\\}$. In this case $=$ is standard equality.

For standard equality we would have that $1=1$ but $1\neq 2$. Pretty much just the normal old equality we are used to.

> **Example**: Let $R(\mod n)$ be the set of all pairs of numbers in $\mathbb{Z}$ that have the same remainder when divided by $n$[^1]. This is notated as $a\equiv b\mod n$.

Let $n=2$. Since both $6$ and $4$ are divisible by $2$, then the remainder is $0$ and we say that
$$
6\equiv 4\mod 2.
$$
However $3$ is not divisible by $2$, so
$$
6\not\equiv 3 \mod 2.
$$

> **Example**: Let $R(\leq)=\\{(x,y)\in\mathbb{R}\times\mathbb{R} | x<y\text{ or } x=y\\}$. This is just normal $\leq$

As is standard, $1\leq 2$ but $1\not\leq 0$.

> **Example**: Let $p,q$ be people. We will say that $(p,q)\in R(\text{isParent})$ if $p$ is a parent of $q$. We will notate this as $p\text{.isParent}(q)$.

Here we are defining a function that relates two items in a way that we could imagine inside of our code! I have no kids[^2] so I would not be Joe.isParent() of anyone, but my mom is my parent so Susan.isParent(Joe) would be true.


[^1]: This is not the technical way to define $\mod n$ but we will do that later when we talk about number theory.
[^2]: As of the time I am writing this page, yerrrrrrr ❌👶 