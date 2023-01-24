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
weight: 210
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
for n in all_integers: 
    # note there are infinite integers,
    # so this is just a nify example thats not real
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
for n in all_integers:
    # note there are infinite integers,
    # so this is just a nify example thats not real
    S.add(n*n)
```

---

### Collectors Edition

We might start to build all sorts of cool crazy sets using the things we learned in the above section by setting $P(x)$ to be anything. For example, we might say $P(x)$ is "$x$ is a NJIT dorm building", or "$x$ is a table" or "$x$ is a hydorgen atom in the universe", and create these abstract sets to work with (in some weird way).

What about sets of sets? Why not! This is a totally valid property to collect sets, and why not have things such as
$$
A = \\{E | E=\\{x\\} \text{where x is even}\\}
$$
which will create us a set that contains smaller sets that contain only one number that is even. There is even a special term[^1] for sets that contain other sets!

>__Definition__: A set whos elements are also sets is a __collection__.

Note that all collections are sets, but not all sets are collections.

---

### Historical Tangent: Can $P(x)$ be Anything?

As of now this might seem amazing and a way to collect literally anything into a set, which is what Gottlieb Frege thought he could do in the late 1800s. Mans literally wanted to restart math using sets and decided on a couple of straighforward axioms, and one of them was the idea that if you could define some property of an object $P(x)$ then you could make a set for it.

Clearly, from our previous examples, we can see that this is pretty easy so what could go wrong here? The question came up of well, if we have sets that contain sets, can a set contain itself recursivly? Our boy Frege was like *"fuck ya why not seems fine to be"* and allowed shit like the following to fly
$$
A = \\{A\\}.
$$
Really just not thinking to deeply about what something like this actually means, seeing as its a bit bizzare, notice that we can say that there is now a property of a set $P(A)$ where either the set contains itself or it doesn't. Lets consider an example of a set that does not contain itself, such as
$$
A = \\{1,2,3\\},
$$
however literally any of our previous examples from other sections also would work. Since a set not containing itself is a property, per the axiom, we should be able to collect them all into a collection of all sets that do not contain themselves
$$
D = \\{A | A\not\in A\\} = \\{\mathbb{N},\mathbb{Z},\phi,\ldots\\}.
$$
Alright thats fine, but wait, $D$ is a set, so does $D$ contain itself or not? Well say that $D$ __does__ contain itself, then $D\in D$, but $D$ is all the sets that __do not__ contain themselves which would mean $D\not\in D$.

Wait ok so that clearly doesn't work, what about if $D$ __does not__ contain itself? In that case then $D\not\in D$ but then that means $D$ should be in $D$! So this doesn't work either, as not matter what happens we reach a contradiction. This means that under this set of axioms we cannot build this set, and as such this axiom is __not true__.

This is known as __Russell's Paradox__ in honor of the guy who figured it out and literally rendered two volumes of work by Frege worthless 😬

[^1]: This term is not frequently used
