---
title: What is a Set?
linktitle: What is a Set?
toc: true
type: book
date: 2023-01-16
draft: false
tags:
    - cs241
    - sets

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 205
---

## Screw Writing Out Lists

When you look at the NJIT campus, there are a bunch of dorm buildings. Specifically, there is *Oak, Cyprus, Redwood, Maple, Laurel, Greek Village,* and *Honors*; NJIT sure likes its trees huh. Suppose you are talking to someone about the dorms on campus, are you going to list out all of them every time you need to mention the group? Obviously not, that would be stupid and time consuming; instead you would talk using a phrase such as *"the dorms"* where *"the dorms"* represents some abstract collection that contains all the dorm buildings. This collection doesn't have an order, because its just *"the dorms"*, an order wouldn't make sense. This collection could also be easily checked for membership, such as

>"Is honors in 'the dorms'?"
>
>"Yeah dude its a dorm here."
>
>"Is Apple in 'the dorms'?"
>
>"No? Do you even go here?"

This abstract collection is what we call a __set__, and makes up the backbone of foundational mathematics.

> __Definition__: A __set__ is an unordered collection of unique objects, denoted with curly braces $\{\}$.

For the purposes of notation, we will represent sets usually with capital letters $A,B,C,\ldots$ and (variable) items in sets with lowercase letters $a,b,c,\ldots$.

> __Definition__: If $a$ is an item inside set $A$ then we say $a$ is an __element__ of $A$. $a$ is a __member of__ A, and this relationship is notated as $a\in A$.

When we defined sets, we said that was unqiue. This does not mean that repeated elements are dropped from our set, rather they are only included once. For example, suppose I took a list of ages of students I have in class. Something like
$$
[19,19,20,21,24,28,25,19,17,33,\ldots].
$$
If I was to ask *"What ages are present in our class?"* you would rightfully reply *"$17,19,20,21,24,28,33,\ldots$"* excluding the repeats. This is not because we remove them, but rather the only thing we care about whether an item is a member, or not a member of our set.

### Subsets

Think about what the word "subset" means when you say it. *"This is an issue facing a subset of NJIT students"* means that some quantity of NJIT students are experiencing this issue, and every student facing the issue is an NJIT student[^1]. This term derives from a mathematical term that we use to relate two sets together

> __Definition__: Set $B$ is a __subset__ of A, notated $B\subseteq A$, iff every element of $B$ is an element of $A$. If $A\neq B$ then $B$ is a __proper subset__ of $A$ and is notated $B\subset A$.

In math notation, we would say
$$
B\subseteq A\iff \forall b\in B\implies b\in A
$$
and
$$
B\subset A\iff \left(\forall b\in B\implies b\in A\right)\land\left(\exists a\in A,s.t. a\not\in B\right).
$$

Think of $\subseteq$ similarly to $\leq$, and $\subset$ like $<$. Normally we will use $\subseteq$ unless we need to otherwise.

### Equal Sets

## Important Sets

[^1]: This phrase can be used a bit liberally so just go with it.
