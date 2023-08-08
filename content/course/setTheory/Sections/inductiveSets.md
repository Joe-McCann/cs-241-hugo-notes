---
title: Generating Infinite Sets
linktitle: Inductive Method
toc: true
type: book
date: 2023-01-24
draft: false
tags:
    - cs241
    - set theory
    - induction

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 225
---

{{% callout warning %}} 
This page needs some work done to it, as I feel its quality and explanations are not very good. Feel free to skip it for now
{{% /callout %}}

## First Looks into Infinity

The content of this section is going to appear pretty theoretical for the sake of nothing, but will hopefully make it so that a future idea that we cover will not be so confusing. This material comes from and is inspired by topics from the Munkres Topology first chapter[^1].

We are going to go a little off the rails to describe a few sets and how we can build up the entirety of the integers and more. This shit is going to be seriously non-standard as I'm going to use a lot of my own terminology here (the book is weird and I'm modifying it for our CS use cases), but I swear it may possibly assist slightly in making Induction Proofs not so fucking confusing in the future.

### Inductive Sets

Lets imagine the following game, pick some numbers to have in a set of numbers. Then add $1$ to all the numbers in the set and insert them into the set. Repeat this process infinitely. I suppose this isn't actually a game, as its

1. Without a goal
2. Not fun in the slightest

but whatever, we run thought experiments. For example, lets start with a set that contains $5, 8$ and then see what happens
$$
\begin{aligned}
A_0&=\\{5,8\\} \\\\
A_1&=\\{5,6,8,9\\} \\\\
A_2&=\\{5,6,7,8,9,10\\} \\\\
A_3&=\\{5,6,7,8,9,10,11\\} \\\\
\vdots&
\end{aligned}
$$

As we can see, we are continually adding more and more numbers, and this process appears to go on until infinity. In fact, for every number $n\geq 5$, we can see that $n+1$ will also be inside our set, as at the next step we would just add $1$!

In the book I am referencing they call this an *inductive set*[^2] but I personally dislike this term (and nobody uses it), so we will consider this more of a method that we use to create sets, rather than a set itself.

> **Definition**: Select a set $A_0$ and function $f(x)$[^3]. For every $n$, define $A_{n+1}$ as the set created by taking the union of $A_n$ and the function applied to all items of $A_n$. This is an **inductive method**. In set notation $$A_{n+1}=A_n\cup\bigcup_{x\in \mathcal{P}(A_n)} f(x)$$

For our previous example we had that $A_0=\\{5,8\\}$ and $f(x)=x+1$. At every step of the way we are taking the numbers and adding $1$ to them infinitely.

{{% callout info %}}
In our examples we are defining $f(x)$ to take in a number as an input, and return a number as an input. In the general applications shown later $x$ can be a subset of $A_j$ and output a set of numbers.
{{% /callout %}}

We could also in theory have decided to make $f(x)$ the rule where for every number you multiply by $2$, and you multiply by $3$, to give
$$
\begin{aligned}
A_0&=\\{5,8\\} \\\\
A_1&=\\{5,8,10,15,16,24\\} \\\\
\vdots&
\end{aligned}
$$
This example is a silly rule, but shows how you can get real creative with whatever you want to do. However, each of these individual sets isn't really useful, because we want to know what happens in the limit as we head off to infinity instead of dealing with $A_{10,000}$ or other really big numbers.

> **Definition**: $A$ is an **limiting set** of an inductive method iff
>
> 1. $\forall j\geq 0, A_j\subseteq A$
> 2. $\forall x\in A, \exists n$ s.t. $\forall j\geq n,x\in A_j$
>
> This is written as $$\lim_{j\rightarrow\infty}A_j=A$$

Congrats! You literally just theoretically defined a limit of sets in terms of foundational mathematics. Now we can make assertions about the limiting set of our process, which will eventually help us out.

> **Theorem** <a name="standard_induction_theorem">(Standard Induction Theorem)</a>: Let $A_0={0}$ and $f(x)=x+1$, The limiting set of the inductive method defined by $A_0, f(x)$ $$\lim_{j\rightarrow\infty}A_j = \mathbb{N}
<details>
  <summary>Proof</summary>
  Proof: Let us assume for the sake of argument that
  $$
    \lim_{j\rightarrow\infty} A_j = A \neq\mathbb{N}.
  $$
  Since every $A_j\subset\mathbb{N}$, we know $A\subseteq\mathbb{N}$. This would mean that there must exist an element of $\mathbb{N}$ that is not inside of the limiting set $A$. Let us call the smallest number that is missing from $A$, $n$. Clearly $n\neq 0$, as we stated that $0\in A_0$.
  </br>
  If $n$ is the smallest missing number from $A$, this means that $n-1\in A$ since $n>0$. However, we know that if $n-1\in A$, then $n\in A$ as well, as $f(n-1)=n$. This contradicts that $n$ is the smallest missing number, and as such we know that $A=\mathbb{N}$.
  </br>
  <b>Q.E.D.</b>
</details>

A similar claim can be made that if $A_0=\\{1\\}, f(x)=x+1$ then
$$
\lim_{j\rightarrow\infty} A_j = \mathbb{Z}_+.
$$

---

### Stronger Inductive Methods

Lets consider some more challenging examples of ways to generate $\mathbb{N}$. Lets go through an idea in which $A_0=\\{0,1\\}$ and $f(x)=x+2$.
$$
\begin{aligned}
A_0&=\\{0,1\\} \\\\
A_1&=\\{0,1,2,3\\} \\\\
A_2&=\\{0,1,2,3,4,5\\} \\\\
A_3&=\\{0,1,2,3,4,5,6,7\\} \\\\
\vdots&
\end{aligned}
$$
Notice that for every individual number we are only adding $2$, which means that we are constantly skipping numbers, but since we have our $A_0$ contain *both* $0,1$, those two initial conditions cover the entirety of $\mathbb{N}$! We can do the following

> **Theorem**: Let $A_0=\\{0,1,2,3,\ldots,k-1\\}, f(x)=x+k$. $$\lim_{j\rightarrow\infty}A_j = \mathbb{N}.
<details>
  <summary>Proof</summary>
  Proof: The proof goes pretty much exactly the same as the previous one.
  </br>
  </br>
  Let us assume for the sake of argument that
  $$
    \lim_{j\rightarrow\infty} A_j = A \neq\mathbb{N}.
  $$
  Since every $A_j\subset\mathbb{N}$, we know $A\subseteq\mathbb{N}$. This would mean that there must exist an element of $\mathbb{N}$ that is not inside of the limiting set $A$. Let us call the smallest number that is missing from $A$, $n$. Clearly $n>k-1$, as we stated that $A_0=\{0,1,\ldots,k-1\}$.
  </br>
  If $n$ is the smallest missing number from $A$, this means that $n-k\in A$ since $n>0$. However, we know that if $n-k\in A$, then $n\in A$ as well, as $f(n-k)=n$. This contradicts that $n$ is the smallest missing number, and as such we know that $A=\mathbb{N}$.
  </br>
  <b>Q.E.D.</b>
</details>

This shows that so long as you have enough initial cases, you can skip over as many finite steps as you'd like! Lets make it even more general here.

> **Theorem** <a name="strong_induction_theorem">(Strong Induction Theorem)</a>. Let $A_0=\\{0\\}$ and $f(\\{0,1,\ldots, j\\})=j+1$, also notated as $f(A_j)=j+1$. $$\lim_{j\rightarrow\infty} A_j = \mathbb{N}.

The proof of this is near identical to the other two we've done, so I'll skip it for the sake of brevity. Also I'm cranking out these notes right before class and would like to finish in time lmfao.

In case that description is confusing, what we are doing is instead of just plugging in one number and getting out some formula of that, we plug in the *entire set* that goes from $0$ to $j$ in order to get the next number. This might seem like a more restrictive, shittier version of the original inductive set, but when we get to proof by induction it actually will show to be quite helpful.

---

## Examples

> **Theorem**: Let $A_0=\\{0\\}$ and $f(x)=\\{2x, 2x+1\\}$. $$\lim_{j\rightarrow\infty} A_j=\mathbb{N}

[^1]: Munkres, J. Topology James Munkres Second Edition.
[^2]: Inductive Set is actually an abused term, as it appears many authors have [different uses](https://mathworld.wolfram.com/InductiveSet.html) for it, and in the modern frame of mathematics nobody really uses it at all. I am including it solely because I think it will assist with understanding induction by adding an additional step.
[^3]: The function should actually take in a set and return a set, as to make this more general. As for why, that will be explained in the functions section
