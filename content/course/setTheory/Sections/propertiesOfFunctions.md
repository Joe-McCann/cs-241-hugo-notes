---
title: Properties of Functions
linktitle: Properties of Functions
toc: true
type: book
date: 2023-01-24
draft: false
tags:
    - cs241
    - functions
    - function properties

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 235
---

## Some of Math's Coolest Terms

We have the general idea of functions, but as with anything in math, we can define some useful properties to have.

### Injectivity

We have as a propery of functions that every input must have exactly one output, but what if that output is always unique to the given input? What I mean is that it is not possible for two different inputs to give the same output? This would be called injective (or one-to-one) and is our first property.

> **Definition**: A function $f(x)$ is **injective** if for all $x,y$ $f(x)=f(y)$ iff $x=y$.

What this is saying is that the *only way* we can have the function give the same output, is if the function has the same input! Lets say that we have sets $A=\\{1,2,3\\}$ and $B=\\{1,2,3,4\\}$ and define function $f(x)$ to be

$$
\begin{aligned}
f(1)&=3 \\\\
f(2)&=2 \\\\
f(3)&=4.
\end{aligned}
$$

We can see that $f$ is injective because the same output is never repeated. Here is an example of a non injective function.

> **Example**: Let $\sin:\mathbb{R}\rightarrow\mathbb{R}$. Prove $\sin(x)$ is not injective
<details>
<summary>Proof</summary>
In order to show that this is not injective, we just need to show that we can find two different inputs of $\sin$ that have the same output. Since we know this is a $2\pi$ periodic function its easy to show by seeing that $\sin(0)=\sin(2\pi)=0$. Therefore $\sin$ is not injective.
</details>

Notice that since injective means that every item in $A$ has a unique output of $B$, we can get this nice little small theorem

> **Theorem**: Let $A$, $B$ be sets. If there exists an injective $f:A\rightarrow B$, then $|A|\leq|B|$.

### Surjectivity

Think back to my example about the function `getDog` that takes in a person, and returns back the dog that they own. Now suppose you found a dog, is it true that you can always find a person such that this dog will be the output of `getDog`? If that seemed a little obtuse suppose the dog you found is named `Max`. Are you $100\%$ sure that you can find a person $p$ such that `getDog(p)=Max`, or that `Max` has an owner?

Sadly no, that is not guaranteed because sometimes dogs just don't have owners, whether they are a rescue or a stray. This means that while our function takes in people and maps them to dogs, it doesn't map them to *every* dog, there are still dogs missing. We can also say that in this case $codomain(f)\neq im(f)$ (hence why we need the two terms).

What happens though if we say that every dog *was* covered? That would be the idea behind a surjective (onto[^1]) function.

> **Definition**: A function $f:A\rightarrow B$ is **surjective** if $\forall b\in B, \exists a\in A$ such that $f(a)=b$.

As in, no matter what output you select from your codomain, you can always find an input that maps to it. This is equivilent to

> **Theorem**: A function $f$ is surjective iff $codomain(f)=im(f)$.

> **Example**: Let $f:\mathbb{R}\rightarrow\mathbb{R}$, $f(x)=x\cdot\cos(x)$. Prove $f$ is surjective.
<details>
<summary>Proof</summary>
Suppose we choose some value $y\in\mathbb{R}$ and want to see if we can find some value $y=x\cos(x)$. Note that $x\cos(x)$ is continuous, so by the intermediate value theorem, if I have two points, $f$ will take all values of $y$ between those two points. If you don't remember calculus, just remember that since $f$ is continuous, if I want to get from $a$ to $b$ without lifting my pen, I have to cross every point between $a$ and $b$.
</br>
Remember that $\cos(x)$ is $1$ whenever you are at a value of $x=n\pi$ whenever $n$ is even, and is $-1$ whenever $n$ is odd. So if $y &lt n\pi$, we know that $f(n\pi) = n\pi \cdot 1 > y$
</details>

Similar to the previous section, if we know that there are enough items in $A$ to completely cover $B$, then there must be at least as many items in $A$ as in $B$.

> **Theorem**: Let $A$, $B$ be sets. If there exists a surjective $f:A\rightarrow B$, then $|A|\geq|B|$.

### Bijectivity

Bijective functions are super easy if you understand surjective and injective, because its just both!

> **Definition**: A function $f(x)$ is **bijective** iff it is injective and surjective

Bijections can be thought of as a perfect mapping, or relabeling. This means that we can take every item in each of our sets and pair them with each other, without leaving anything out or mixing them up. This makes it pretty clear the sets should be the same size, and by applying the two theorems from previous sections we can see that.

> **Theorem**: Let $A$, $B$ be sets. If there exists a bijective $f:A\rightarrow B$, then $|A|=|B|$.

> **Example**: Let $f:\mathbb{R}\rightarrow\mathbb{R}$, $f(x)=x^3(x)$. Prove $f$ is bijective.

## Bijective is the Bestjective

[^1]: "Onto" is such a dumb fucking term
