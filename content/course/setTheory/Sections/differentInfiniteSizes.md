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
where each term in the sequence adds a new digit of $\pi$. Clearly this is an infinite sequence of rationals (and by extension reals) that converges to a number that is not rational. If we change this sequence at **any single digit** though we'll converge to a brand new irrational number, and then we can change that sequence somewhere, and that sequence. There are an infinite amount of infinite sequences in which we have infinite choices to change, and the fact that there are infinite reals and irrational numbers which have infinite non-repeating digits is the key to our proof.

Before we prove the main theorem though, let us first show a quick mini-theorem we will use to help us

> **Lemma**: $|\mathbb{R}|=|(0,1)|$

{{% callout info %}}
<details>
<summary>Proof</summary>
Consider the function $f:\mathbb{R}\rightarrow (0,1)$ as $f(x) = \frac{\arctan(x)}{\pi}+\frac{1}{2}$. This function is bijective as it has inverse function on this domain $f^{-1}(x)=\tan\left(\pi x-\frac{\pi}{2}\right)$
</br>
QED
</details>
{{% /callout %}}

With this info, instead of solving out the uncountability theorem as its written, we can instead show that $|\mathbb{N}|<|(0,1)|$ since $|\mathbb{N}|=|\mathbb{Q}|$ and $|\mathbb{R}|=|(0,1)|$.

### Cantor's Diagonal Proof

This was not the first way Georg Cantor proved this result, however his first proof was a bit more confusing so mathematicians at the time bullied him so hard that he came up with one of the most iconic proofs in all of math history thinking it would make them believe him[^1].

In order to show that $|\mathbb{N}|<|(0,1)|$, we just need to show that if we have any function $f:\mathbb{N}\rightarrow (0,1)$, that $f$ will **never** be surjective. Remember that surjective means that the function completely covers the codomain with the domain, so if $f$ can never be surjective, that means that no function can ever hit every number in $(0,1)$. This would prove the claim.

For the sake of argument (and eventually contradiction), assume that we have have some function $f$ that is surjective. This means that if I plug in every natural number, I will eventually get out **every** number in $(0,1)$. Just to visualize, lets plug in natural numbers and write out what our function could in theory output.
$$
\begin{align}
f(0)&=.\color{red}{4}\color{white}58429\ldots \\\\
f(1)&=.5\color{red}3\color{white}8828\ldots \\\\
f(2)&=.19\color{red}5\color{white}820\ldots \\\\
f(3)&=.775\color{red}9\color{white}37\ldots \\\\
f(4)&=.9999\color{red}9\color{white}1\ldots \\\\
f(5)&=.96592\color{red}9\color{white}\ldots \\\\
&\vdots
\end{align}
$$

Remember that every input is a natural number, and every output is a real number in $(0,1)$. Also, the actual digits we used here don't matter, we could have chosen any this is just for visualizing. However, since $f$ is surjective, **every value in $(0,1)$ must be somewhere in this output list**. This is a **very important** piece of the proof, so please understand before moving on.

Now notice that I highlighted specific decimal digits of each number in red. Specifically, for $f(n)$ I highlighted the $n^{\text{th}}$ digit (if we start at $0$), and do this infinitely for every $f(n)$. We are going to create a new number $x$ which we will define as follows: the $n^{\text{th}}$ digit of $x$ is the $n^{\text{th}}$ digit of $f(n)$. In easier terms, we are creating $x$ by taking all the digits that we highlighted, so in our example
$$
x = .435999\ldots
$$

See that by how we defined $x$, its $n^{\text{th}}$ digit is always the same as $f(n)$ $n^{\text{th}}$ digit. Lets create a brand new number $y$ by taking $x$ and modifying it a bit: for every digit of $x$, add $1$, and if the digit is $9$, make it $0$. So since for us we know $x=.435999\ldots$, we can see that
$$
y = .546000\ldots
$$

Think about the value $y$ for a second. By definition, $y$ is different than $x$ in every single decimal place. But remember that we said that $x$ has the same $n^{\text{th}}$ digit as $f(n)$. This means that $y$ **must** have a different $n^{\text{th}}$ digit than $f(n)$ for **every** value of $n$. But wait, this means now $y$ actually cannot be an output of $f(n)$, because $y$ differs in at least one decimal point for every value of $n$! This means that $f$ cannot be surjective which contadicts our point that it was, therefore no surjective functions can exist!

As such $|\mathbb{N}|<|(0,1)|$ and by extension $|\mathbb{Q}|<|\mathbb{R}|$. **Q.E.D.**

For those of you who take Analysis and measure theory in the future, it turns out that there are way, *way*, more real numbers than rational numbers. In fact, if you were to take the "percentage"[^2] of rationals vs reals, you would find that the rationals make up $0\%$ of the real numbers! This might seems fairly abstract, but there are real consequences to this!

## A Real World Consequence of Uncountability for Computer Scientists

[^1]: It didn't
[^2]: Theres more technical theory here I'm just grazing over it for brevity