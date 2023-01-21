---
title: Building Sets
linktitle: Set Builder Notation
toc: true
type: book
date: 2023-01-16
draft: false
tags:
    - cs241
    - set theory
    - set builder

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 205
---

## Making Sets Even More Brief

Look at an example of a set

>__Example__: Let $\mathbb{D}$ be the set of all NJIT dorm buildings.

This is way better than writing out every individual building, but its still pretty verbose. What we want (because we are lazy mathematicians) is to be able to make things even more consise, *especially* when dealing with mathimatical things. This is where we need __set builder notation__.

>__Definition__: __Set Builder Notation__ is a way of writing out sets in a way that generates the set based on some conditions. Written as $$A=\\{f(x)|P(x)\\}$$ where $A$ is the set, $f(x)$ is the item inserted into the set as a function of $x$, and $P(x)$ is a condition to insert $f(x)$.

Note that in set builder notation $|$ means "such that", although its often replaced with by $:$ to avoid confusing notation. 

In order to write out our dorm example, we could just say that

$$
\mathbb{D} = \\{d|\text{d is an NJIT dorm}\\}.
$$

Set builder notation is actually just a loop that we use to insert items into sets. For example, suppose I wanted to write out a set that contains all numbers larger than $10$, I could say now that

$$
A = \\{n|n>10, n\in\mathbb{Z}\\} = \\{11,12,13,\ldots\\}
$$
although sometimes it will be skipped that $x\in\mathbb{Z}$ if its clear what number system you're using. People will also sometimes add the domain that we are selecting from in the start because that how we'd linguistically define our sets, but I prefer it in the back half. This set builder notation is literally just like looping, we could think of it like

```python
A = set()
for n in all_integers: #note there are infinite integers, so this is just a nify example thats not real
    if n > 10:
        A.add(n)
```

Or if we wanted to create a set in which it contained all the square numbers, we could say that
$$
S = \\{n^2 | n\in\mathbb{Z}\\} = \\{0,1,4,9,\ldots\\},
$$

which as a loop is

```python
S = set()
for n in all_integers: #note there are infinite integers, so this is just a nify example thats not real
    S.add(n*n)
```

### Can $P(x)$ be Anything?

We might start to build all sorts of cool crazy 