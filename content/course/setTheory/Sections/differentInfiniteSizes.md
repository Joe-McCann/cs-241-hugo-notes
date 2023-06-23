---
title: Larger Size of Infinity
linktitle: Larger Size of Infinity
toc: true
type: book
date: 2023-06-17
draft: false
tags:
    - cs241
    - sets
    - infinity
    - functions

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 255
---

## The Rationals and the Reals

On the previous page we showed (kinda) how the rationals $\mathbb{Q}$ was a countable set the same size as the naturals $\mathbb{N}$. Right now we will take some time to how $\mathbb{Q}$ is quite intertwined with the set of real numbers $\mathbb{R}$. As we know, $\mathbb{Q}\subset\mathbb{R}$, but there are some numbers in $\mathbb{R}$ that are *irrational* and not in $\mathbb{Q}$, for example $\pi$. From this we can see pretty clearly that $|\mathbb{Q}|\leq |\mathbb{R}|$. The proof of the true relation is pretty involved though, so let's consider some things that might assist our intuition.

### Density

Lets begin by looking at a theorem that should be fairly obvious

> **Theorem**: Let $a,b\in\mathbb{Q}$ and $a\neq b$. Then there exists a $x\in\mathbb{R}$ such that $a<x<b$

This theorem is saying that between any two rational numbers, we can find a real number. Intuitively this should be pretty obvious as any rational number itself is a real number, so we can just take the average of these two rationals

{{% callout info %}}
<details>
<summary>Proof</summary>
Consider $x=\frac{a+b}{2}$. Since adding two fractions and dividing by $2$ will just give another fraction, then we know that $x\in\mathbb{Q}$ which by extension means that $x\in\mathbb{R}$
</br>
QED
</details>
{{% /callout %}}

What is more interesting is the following theorem that says if I pick any two real numbers, I can find a rational in between them!

> **Theorem**: Let $x,y\in\mathbb{R}$ and $x\neq y$. Then there exists a $a\in\mathbb{R}$ such that $x<a<y$

So far we've seen some weird stuff come from infinity so this shouldn't feel too strange, but lets actually prove it

{{% callout info %}}
<details>
<summary>Proof</summary>
To begin, if $x<0$ and $y>0$ then we can choose $a=0$. We will prove this for $x,y>0$ but the proof can be modified for the case in which they are both less. </br>
Let $\varepsilon=y-x$, and choose $n\in\mathbb{N}$ such that $\frac{1}{n}< \varepsilon$ (the fact we can do this is called the Archimedean property). Now let $m\in\mathbb{N}$ be the smallest value of $m$ such that $\frac{m}{n} < x$. Since $\frac{1}{n}< \varepsilon=y-x$, then we know that $\frac{m}{n}+\frac{1}{n} < y$. But we know that $m$ was the smallest value such that $\frac{m}{n}< x$ so $\frac{m+1}{n}>x$. Let $a=\frac{m+1}{n}$ which implies $x< a< y$.
</br>
QED
</details>
{{% /callout %}}

Ok cool, so we can see that picking any two real numbers will have a rational between, and any two rationals will have a real in between. You might now think that this means the number line looks like *rational-real-rational-real-...*, which would totally be a valid way to think, however, the truth of the matter is that you are about to encounter one of the craziest mathematical results in history

## The Reals and Uncountability

One of the most famous theorems in all of mathematics has to do with the cardinality of the reals being *uncountable*

>**Theorem Uncountability of Reals:** $|\mathbb{Q}|<|\mathbb{R}|$

Wait uh what the fuck? How is this possible we literally showed that between two reals there is a rational?? Yep, this is one of those mind boggling results that is tough to wrap your brain around. One way to envision why this might be true is to think of the sequence of rational numbers
$$
3, 3.1, 3.14, 3.141, 3.1415,\ldots
$$
where each term in the sequence adds a new digit of $\pi$. Clearly this is an infinite sequence of rationals (and by extension reals) that converges to a number that is not rational. If we change this sequence at **any single digit** though we'll converge to a brand new irrational number, and then we can change that sequence somewhere, and that sequence. There are an infinite amount of infinite sequences in which we have infinite choices to change, and the fact that there are infinite reals and irrational numbers have infinite non-repeating digits is the key to our proof.