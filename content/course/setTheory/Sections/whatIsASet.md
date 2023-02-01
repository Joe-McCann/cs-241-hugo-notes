---
title: What is a Set?
linktitle: What is a Set?
toc: true
type: book
date: 2023-01-16
draft: false
tags:
    - cs241
    - set theory

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

> __Definition__: A __set__ is an unordered collection of unique objects, denoted with curly braces $\\{\\}$.

For the purposes of notation, we will represent sets usually with capital letters $A,B,C,\ldots$ and (variable) items in sets with lowercase letters $a,b,c,\ldots$.

> __Definition__: If $a$ is an item inside set $A$ then we say $a$ is an __element__ of $A$. $a$ is a __member of__ A, and this relationship is notated as $a\in A$.

When we defined sets, we said that was unique. This does not mean that repeated elements are dropped from our set, rather they are only included once. For example, suppose I took a list of ages of students I have in class. Something like
$$
[19,19,20,21,24,28,25,19,17,33,\ldots].
$$
If I was to ask *"What ages are present in our class?"* you would rightfully reply *"$17,19,20,21,24,28,33,\ldots$"* excluding the repeats. This is not because we remove them, but rather the only thing we care about whether an item is a member, or not a member of our set.

---

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

In the above section, we stated that $B\subset A$ if $B\subseteq A$ and $A\neq B$, but what does it actually mean for two sets to be equal to each other? It turns out that this is actually an [axiom of Set Theory](https://mathworld.wolfram.com/Zermelo-FraenkelAxioms.html)

> __Axiom of Extensionality__: Two sets A, B are equal iff they contain the same elements.

In math notation we would say
$$
\forall x (x\in A\iff x\in B) \iff A=B.
$$

Suppose I asked you to solve the following coding challenge problem:

> __Example__: Write a function that takes in two sets and returns True if the sets are equal and False if otherwise.

If we have two sets it would seem straightforward to loop through the items in the first set $A$ and see if all of those are inside of $B$. However this is not enough, as what if $A=\\{1\\}$ and $B=\\{1,2\\}$? If $A$ is a proper subset of $B$, seeing if everything in $A$ is not enough, we also need to loop through and see if everything in $B$ is also in $A$ too!

```python
def set_equal(A, B):
    for a in A:
        if a not in B:
            return False
    for b in B:
        if b not in A:
            return False
    # If both loops finish then there are no missing items in either set
    return True
```

As such in order for $A=B$, then everything in $A$ is in $B$, so $A\subseteq B$, and by similar logic we know that $B\subseteq A$. With this we can actually get an easy condition to use to prove that two sets are equal to each other, and what we will often use to do so.

> __Theorem__: $A=B$ iff $A\subseteq B$ and $B\subseteq A$
<details>
<summary>Proof</summary>
We will first prove the forward direction that
$$
    A=B \implies A\subseteq B \land B\subseteq A.
$$
Since $A=B$, the two sets contain the same items. As such everything in $A$ is in $B$, so $A\subseteq B$ and by extension $B\subseteq A$.
</br>
Now we will prove the backward direction
$$
    A\subseteq B \land B\subseteq A \implies A=B.
$$
Suppose that $A\neq B$, this would mean that there exists an item in $A$ or $B$ that is not inside of the other. For the sake of argument, suppose there is in item in $B$ that is not in $A$, but that would contradict the idea $B\subseteq A$, therefore there cannot be anything in $B$ not in $A$. Since the same argument can be made for $A$, this direction of the proof is completed.
</br>
<b>Q.E.D.</b>
</details>

This also provides us with another insight into the handling of duplicates, as the sets $\\{1,2\\}$ and $\\{1,2,2\\}$ must be the same set, as all the elements of both of them are in each other.

---

## Important Sets

For sets that are often used or more important, we use bolded fancy looking letters like $\mathbb{A}, \mathbb{B}\ldots$, or fancy cursive letters. In this section I will provide notation for some of the important sets that we will be using throughout this class

1. $\mathbb{N}=\\{0,1,2,3,\ldots\\}$: The natural numbers, all non-negative numbers
2. $\mathbb{Z}=\\{0,1,-1,2,-2,\ldots\\}$: The integers, positive and negative whole numbers
3. $\mathbb{Z}_+=\\{1,2,3,4,5,\ldots\\}$: Positive integers
4. $\mathbb{Q}=\\{0,1,-1,\frac{1}{2},-\frac{5}{7},\ldots\\}$: Rational numbers, all integers and fractions
5. $\mathbb{R}=\\{0,1,-1,\pi, \frac{1}{2}, -e,\ldots \\}$: Real numbers, all numbers on the number line
6. $\mathbb{F}=\\{\text{Jets, Giants, 49ers, Jaguars,} \ldots\\}$: NFL Football teams[^2]
7. $\mathbb{D}=\\{\text{Oak, Honors, Laurel,} \ldots\\}$: NJIT dorm buildings
8. $\varnothing=\\{\\}$: The empty set
9. $U$: The universal set that contains everything (defined per problem basis)

Note that by definition the following holds

> __Theorem__: For every set $A$, $\varnothing\subseteq A$ and $A\subseteq U$.

[^1]: This phrase can be used a bit liberally so just go with it.
[^2]: Yes this is considered important to me lmfao
