---
title: Proof by Induction
linktitle: Proof by Induction
toc: true
type: book
date: 2023-01-07
draft: false
tags:
    - cs241
    - logic
    - proofs
    - induction
    - inductive sets

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 135
---

{{% callout info %}}
This page will use a small amount of information from the chapter on Set Theory. It is not required, but understanding sets might be helpful to get how induction works.
{{% /callout %}}

## Proving an Infinite Amount of Statements

Suppose I want to prove some statement where it has an infinite number of cases (usually do to a $\forall$ quantifier). Since you don't likely have one in mind off the bat, let me provide you the most classic example of a statement we will use induction on.

> **Theorem**: $\forall n\in\mathbb{N}$ $$1+2+3+\ldots+n = \frac{n(n+1)}{2}.$$

Or written out using *sigma notation*

$$
\sum_{k=0}^n k = \frac{n(n+1)}{2}.
$$

We want to be able to show this statement is true for all values of $n$, so $P(0),P(1),P(2),\ldots$ (if we refer to $P(n)$ as the formula for some specific value of $n$). Clearly, we could just plug in the formula and see the pattern

$$
\begin{aligned}
0&=\frac{0(1)}{2} \\\\
1&=\frac{1(2)}{2} \\\\
1+2 &= \frac{2(3)}{2} \\\\
1+2+3 &= \frac{3(4)}{2} \\\\
&\vdots
\end{aligned}
$$

We could go through enough  examples to satisfy that it always works, but as you can see [here](https://math.stackexchange.com/questions/514/conjectures-that-have-been-disproved-with-extremely-large-counterexamples) that is often not enough. I get a laugh out of that thread specifically where they show the first instance of showing that $n^{17}+9$ and $(n+1)^{17}+9$ are not always relatively prime is when $n$ is a $52$ digit number lmao.

Anyway, we do actually have a way to show some infinite theorems are true in a very slick way!

---

## What is Induction?

Say I put everyone in class into a line and had everyone hold hands with the two people next to them. I then say that we are going to play a game, close your eyes and if you ever feel someone let go of one of your hands, you let go of your other hand. So if someone in the middle feels the person to their left let go, they will then proceed to let go to the person to their right.

It is not hard to see how this will cascade into a chain effect, as if one person lets go, that will cause whoever they were holding onto to let go to their other hand, and then the next person will let go, and the next, and the next. In fact, we can see that if any person $p_k$ lets gets their hand let go from, we know that **everyone** down the line from them must also get let go from!

Now instead of our class, imagine that we have an infinite line of people; the same idea still hold. Even if it takes a *very, very* long time, eventually every person down the line from $p_k$ will let go of everyone. If I was the first person on the chain and then told everyone that I would left go of the first person's hand, well before starting I would know that the process would eventually result in everyone letting go, as I was the first person and anyone down the line from me will let go!

With this in mind, we can move into the realm of mathematics. Induction is the idea that if I have some statement $P(n)$ true imply that the next statement $P(n+1)$ is true, then if $P$ is true for any particular $n$, then it must be true for all $n$ down the line. This the idea of "if I know somebody let go, everyone after them will too!". In order to prove by induction you have to show two things

> **Theorem** (Proof by Induction): Suppose that some statement $P(n)$ has the following properties
>
> 1. (Base Step): $P(0)$ is true[^1]
> 2. (Inductive Step): $P(n)$ true implies $P(n+1)$ is true.
>
> Then $P(n)$ is true $\forall n\in\mathbb{N}$.

In order to see a proof of this, see the [Standard Induction Theorem](/course/settheory/sections/inductivesets#standard_induction_theorem).

This means that if we can show that $n=0$ is true, and that any statement implies the next one, then it also means that all of them are true.

---

### Proving With Induction

Now with induction lets walk through the first example we gave at the top of the page

> **Theorem**: $\forall n\in\mathbb{N}$ $$1+2+3+\ldots+n = \frac{n(n+1)}{2}.$$

Clearly our base case of $n=0$ is true, as
$$
0 = \frac{0(1)}{2}.
$$
Now we need to prove the induction hypothesis. Suppose that somewhere out there we found that the statement was true for some particular value of $n$, which would mean that
$$
1+2+3+\ldots+n = \frac{n(n+1)}{2}. \tag{*}
$$
Note that we put the (\*) so that we can refer back to that line in the future. What can we use from this to show about the statement for $n+1$?. Well lets first write down what we want to show. We want to show that
$$
1+2+3+\ldots+n+(n+1) = \frac{(n+1)(n+2)}{2}
$$
is true. Lets start with the left hand side, notice that the selected terms look very familiar
$$
\underbrace{1+2+3+\ldots+n}_{\text{Seem familiar?}}+(n+1).
$$
In fact, everything in the selected region is literally just (\*), and by the inductive hypothesis we assumed that equation to be true, so we can just plug the left hand side of (\*) to get that
$$
\begin{aligned}
& 1+2+3+\ldots+n+(n+1) \\\\
&= \frac{n(n+1)}{2} + (n+1) \\\\
&= \frac{n(n+1)+2(n+1)}{2} \\\\
&= \frac{(n+1)(n+2)}{2}
\end{aligned}
$$
which is what we were trying to show. As such, we know that if $P(n)$ is true for some value of $n$, then it must also be true for $n+1$. Since it is true for $n=0$, we know it is true for all $n\in\mathbb{N}$.

---

### Induction, but Stronger

Prior, we just assumed that $n$ was true in order to prove that $n+1$ was also true, but why limit ourselves there artificially? Why not just assume that every single value up until $n$ was true and use all of those to prove that $n+1$ is true? This is called *strong induction*.

> **Theorem** (Proof by Strong Induction): Suppose that some statement $P(n)$ has the following properties
>
> 1. (Base Step): $P(0),P(1),\ldots,P(k)$ is true for some first $k$ values (dependent on problem)
> 2. (Inductive Step): $0\leq P(k)\leq n$ true implies $P(n+1)$ is true.
>
> Then $P(n)$ is true $\forall n\in\mathbb{N}$.

This one seems more confusing but is actually super intuitive in that we can reach back as far as we want, provided we fulfil the needed base cases. Lets provide an example to see.

> **Theorem**: $\forall n\geq 8$, we can find an $a,b\in\mathbb{N}$ such that $5a+3b=n$.

In other words, we can write any number as a sum of a multiple of $5$ and $3$. Before we do the base case, lets first do the inductive step. Assume that for every value of $n$ up to a certain point, this statement is true. How can we use this to prove $n+1$?

Well we could notice that
$$
n+1 = (n-2)+3.
$$
Since $n-2<n$, we know that by our induction hypothesis, we can write $n=5a+3b$. We can plug this in to find
$$
\begin{aligned}
n+1 &= (n-2)+3 \\\\
&= 5a+3b + 3 \\\\
&= 5a+3(b+1)
\end{aligned}
$$
which satisfies our statement.

For the base case, since we step back by $3$ spaces, we need to show $3$ base cases. We need to show that
$$
\begin{aligned}
8&=5+3 \\\\
9&=3\cdot 3 \\\\
10&= 5\cdot 2.
\end{aligned}
$$
Now this proves our claim. **Q.E.D.**

---

## Practical Applications in Computer Science

The ideas of induction actually have a very direct analog in computer science. If induction is the idea that you can use previous cases to prove the current case that you want to prove, this sounds *exactly* like how a recursive algorithm works.

For recursion, we assume that our code can work a smaller version of our problem, and then use that to solve our current problem. Is this not literally how induction works? The **base case** of a recursive algorithm is even identically the same term as in induction 😆, literally the only difference is recursion is called the recursive step instead of the inductive step.

### Recursion Example

> **Example**: Given a binary tree $T$ and an integer value, write a function that returns **True** if a particular node in the tree contains the value.

When approaching this problem, see that the children of the root node of a binary tree, are also just two binary trees. As such, say we knew how to solve any $n$ height binary tree, with that we could then solve any $n+1$ height binary tree by just seeing if the root node was what we were looking for, and then solving the two $n$ sized children! This algorithm is literally just induction.

For the base case, if you are given an empty tree, then obviously it is not present in that. From this, we can see that our algorithm would be given by

```python
def depth_first_search(root, value):
    if root == None:
        # Empty tree
        return False
    if root.value == value:
        return True
    else:
        # Check left and right sub-trees
        return depth_first_search(root.left, value) or depth_first_search(root.right, value)
```

For a tree of $n$ nodes, this algorithm runs in $\mathcal{O}(n)$ time with $\mathcal{O}(n)$ space for an arbitray tree[^2].

[^1]: We do not need to start with the base case of $P(0)$, we could have picked any other value of $k$, but just note that doing so would leave all cases $0\leq n < k$ unproven.

[^2]: This is $\mathcal{O}(h)$ where $h$ is the height of the tree, but for any tree without restrictions, this is size $n$ in the worst case
