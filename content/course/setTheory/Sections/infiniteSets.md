---
title: Size of Infinity
linktitle: Size of Infinity
toc: true
type: book
date: 2023-05-24
draft: false
tags:
    - cs241
    - sets
    - infinity
    - functions

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 250
---

## What does it mean to count?

When I ask you *"How many apples are there"* depending on however many apples you are counting, you'll mentally provide some number. $1,2,3,4,\ldots$ etc. Counting is counting, you've done it since you were a child.

Say I ask you *"How many items are in the set $\\{a,b,c\\}$?*, you might think *"Uh, obviously $3$ dumbass"*. You would be right, but I have a much more general question, what does it really, *really*, mean for there to be $3$ items in the set? What does it mean to count?

Well for purpose of simplifying, lets say the empty set $\phi$ has $0$ items in it. Then the set that only has $\\{1\\}$ would have one item in it. By extension $\\{1,2\\}$ has two items, $\\{1,2,3\\}$ has three items, and we can continue so on and so forth. How could we extend this idea to sets that are not as straightforward though, like $\\{a,b,c\\}$? Those items aren't numbers so can we even count them?

Well we know that $\\{1,2,3\\}$ has $3$ items, and from before we know that if two sets have a bijection between them, then the two sets have the same cardinality which was our way of defining a "number of items". So if we can find a bijection between our sets, then the two sets must have the same number of items! For example
$$
\begin{align}
f(a)&=1 \\
f(b)&=2 \\
f(c)&=3 
\end{align}
$$
In this case $f:\\{a,b,c\\}\rightarrow\\{1,2,3\\}$ is a bijection so we know they both must be the same size of $3$. Putting this into a nice theorem that we can use, we can say the following

> Theorem: A set $A$ has cardinality $|A|=n$ iff there exists an $$f:A\rightarrow \\{1,2,\ldots, n\\}$ where $f$ is bijective.

This is well and good, but it gets very interesting once we move into the land of the infinite.

## Infinite Cardinalities

Now that we can compare the sizes of sets and know what it means for a set to be a certain size (defining "size" as cardinality), we can reasonably ask the question *"How big are infinity sets?"*

Well pretty clearly they are bigger than any finite set, because an infinite set will just keep going forever and a finite set will not, but can we compare them to each other?

### Evens and Odds

Let the even numbers be $E=\\{2,4,6,8,\ldots\\}$ and the odd numbers be $\\O=\\{1,3,5,7,\ldots\\}$. Are there more even numbers, odd numbers, or are they the same? Putting this in math notation, which of the following is true?
$$
\begin{align}
|E|&>|O| \\
|E|&<|O| \\
|E|&=|O|.
\end{align}
$$

Intuition might tell you that they should be the same, as after every odd number there is an even number and vice versa, but lets see if we can construct a bijection to prove this.

> Theorem: $|E|=|O|$

{{% callout info %}}
<details>
<summary>Proof</summary>
Consider the function $f:E\rightarrow O$ where
$$
f(x)=x-1.
$$
This function is injective as every even number subtracted by $1$ will be unique, and it is surjective as every odd number has an even above it. You could have also just showed $x+1$ was an inverse. Since $f$ is bijective, then $|E|=|O|$.
</br>
QED
</details>
{{% /callout %}}

The intuition was right[^1]! You might have noticed though that we started our even numbers with $2$, and well $0$ is an even number, so does that impact anything?

Lets say that $E_2=\\{0,2,4,6\ldots\\}$. Now we have that $E\subset E_2$, so will $|E_2|>|E|=|O|$?

Lets consider a function $f:O\rightarrow E_2$ where $f(x)=x-1$. You might notice that $f$ is exactly the same as our previous proof, and by the same logic it actually turns out that $|E_2|=|O|=|E|$! This is crazy as we have a proper subset being the same cardinality as its superset! In essense here when we add $0$ into $E_2$, we are effectively "pushing" all the items down one, but that is ok because there is an infinite amount of them!

If you think that's crazy, just watch this though!

### The Naturals and Countability

[^1]: Unless you said otherwise but I won't blame you infinity is tough 😝