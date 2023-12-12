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
f(a)&=1 \\\\
f(b)&=2 \\\\
f(c)&=3 
\end{align}
$$
In this case $f:\\{a,b,c\\}\rightarrow\\{1,2,3\\}$ is a bijection so we know they both must be the same size of $3$. Putting this into a nice theorem that we can use, we can say the following

> Theorem: A set $A$ has cardinality $|A|=n$ iff there exists an $$f:A\rightarrow \\{1,2,\ldots, n\\}$$ where $f$ is bijective.

This is well and good, but it gets very interesting once we move into the land of the infinite.

---

## Infinite Cardinalities

Now that we can compare the sizes of sets and know what it means for a set to be a certain size (defining "size" as cardinality), we can reasonably ask the question *"How big are infinity sets?"*

Well pretty clearly they are bigger than any finite set, because an infinite set will just keep going forever and a finite set will not, but can we compare them to each other?

---

### Evens and Odds

Let the even numbers be $E=\\{2,4,6,8,\ldots\\}$ and the odd numbers be $O=\\{1,3,5,7,\ldots\\}$. Are there more even numbers, odd numbers, or are they the same? Putting this in math notation, which of the following is true?
$$
\begin{align}
|E|&>|O| \\\\
|E|&<|O| \\\\
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

Let's say that $E_2=\\{0,2,4,6\ldots\\}$. Now we have that $E\subset E_2$, so will $|E_2|>|E|=|O|$?

Let's consider a function $f:O\rightarrow E_2$ where $f(x)=x-1$. You might notice that $f$ is exactly the same as our previous proof, and by the same logic it actually turns out that $|E_2|=|O|=|E|$! This is crazy as we have a proper subset being the same cardinality as its superset! In essense here when we add $0$ into $E_2$, we are effectively "pushing" all the items down one, but that is ok because there is an infinite amount of them!

If you think that's crazy, just watch this though!

---

### The Naturals and Countability

Let's consider how the natural numbers $\mathbb{N}$^[2] interact in terms of size now with the even numbers that we just showed. For the sake of brevity, for this problem we will say that $E$ includes $0$. Clearly since
$$
\mathbb{N}=\\{0,1,2,3,4,5,6,7,\ldots\\}=E\cup O
$$
You would be tempted to say that $|\mathbb{N}|>|E|$, however what happens when we introduce the function
$$
\begin{align}
f&:\mathbb{N}\rightarrow E \\\\
f(n)&= 2n
\end{align}
$$

This function is injective, as
$$
f(n_1)=f(n_2)\implies 2n_1=2n_2\implies n_1=n_2.
$$
It is also surjective as given any even number, we can just divide by $2$ to find a value of $n$ such that $f(n)$ equals the given even. Since $f$ is surjective and injective, it must be bijective too!

But hold on a minute buckaroo, that means then that $|\mathbb{N}|=|E|$??? 

Yes it does! In fact it also implies that $|\mathbb{N}|=|E|=|O|$! If we were to write out the function $f$, we would see this sort of pattern
$$
\begin{align}
0&\rightarrow 0 \\\\
1&\rightarrow 2 \\\\
2&\rightarrow 4 \\\\
3&\rightarrow 6 \\\\
4&\rightarrow 8 \\\\
&\vdots
\end{align}
$$
It almost looks like we are counting the even numbers, such that if I asked you for the the fourth even number was, you could tell me that it was $8$ (starting with $0^{\text{th}}$). We will soon see this is a fundemental quantity and give a name for it.

> **Definition**: Let $A$ be a set where $|A|=|\mathbb{N}|$. We say that $A$ is **countably infinite**, or **countable** for short, and is notated as $|A|=\aleph_0$

Woah what is that symbol $\aleph_0$? So I've heard, the story goes that Georg Cantor after working on infinite cardinalites wanted to come up with a symbol for $|\mathbb{N}|$ but believed greek letters were too overused. As such he decided to use the hebrew letter *Aleph* to represent the cardinality of the naturals, sometimes called *aleph null*. Spoiler alert, using Hebrew letters did not catch on and mathematicians continued to oversaturate Greek letters, oops.

There is a very interesting result involving $\aleph_0$ and how fundemental it is, but I will get to that later. For now lets prove a series of small results that lead up to something cool.

> **Lemma**: Let 
$$
\mathbb{Z}_+=\left\\{1,2,3,4,5,\ldots\right\\}
$$ 
then $|\mathbb{N}|=|\mathbb{Z}_+|$.

{{% callout info %}}
<details>
<summary>Proof</summary>
Consider the function $f:\mathbb{N}\rightarrow \mathbb{Z}_+$ where
$$
f(x)=x+1.
$$
This function is bijective in the same way as the previous examples.
</br>
QED
</details>
{{% /callout %}}

> **Lemma**: Let $\mathbb{Z}_-=\\{-1,-2,-3,-4,\ldots\\}$. $|\mathbb{Z}_-|=|\mathbb{Z}_+|$.

{{% callout info %}}
<details>
<summary>Proof</summary>
Consider the function $f:\mathbb{Z}_+\rightarrow \mathbb{Z}_-$ where
$$
f(x)=-x.
$$
This function is pretty clearly bijective as we are just taking the items and adding a negative sign lmao.
</br>
QED
</details>
{{% /callout %}}

Notice how now we have a lot of sets that all have the same cardinality, $|\mathbb{N}|=|E|=|O|$ and these are also equal to the positive and negative integers. This actually translates, so we have that the set of odd numbers and negative integers are the same size for example. Lets continue with this example for a second and write out some pairings
$$
\begin{align}
-1&\rightarrow 1 \\\\
-2&\rightarrow 3 \\\\
-3&\rightarrow 5 \\\\
-4&\rightarrow 7 \\\\
&\vdots
\end{align}
$$
but wait a second, notice that we can match up all the negative numbers with all the odd numbers. Earlier on we showed that
$$
\begin{align}
0&\rightarrow 0 \\\\
1&\rightarrow 2 \\\\
2&\rightarrow 4 \\\\
3&\rightarrow 6 \\\\
4&\rightarrow 8 \\\\
&\vdots
\end{align}
$$
which means that we can match up all the positive numbers with all the even numbers. What if we combined these two lists together though
$$
\begin{align}
0&\rightarrow 0 \\\\
-1&\rightarrow 1 \\\\
1&\rightarrow 2 \\\\
-2&\rightarrow 3 \\\\
2&\rightarrow 4 \\\\
-3&\rightarrow 5 \\\\
3&\rightarrow 6 \\\\
-4&\rightarrow 7 \\\\
4&\rightarrow 8 \\\\
&\vdots
\end{align}
$$
On the left hand side we are getting all the negative and positive integers, which is just $\mathbb{Z}$, and on the right we find that we have all the natural numbers $|\mathbb{N}|$. By combining these two functions, we have actually created a bijection between the integers and naturals! This means that the following theorem is true.

> **Theorem**: $|\mathbb{N}|=|\mathbb{Z}|$

This is kind of crazy, because even though the integers are infinite in two directions, we can still find a way to match everything up. For a set to be *countable*, what we effectively need is to find a way to list out every item of the set in a specified order, as then you can match up the naturals.

---

### The Rationals and Countability

Remember that the set of rational numbers $\mathbb{Q}$ is the set of all fractions $\frac{a}{b}$ where $a,b\in\mathbb{Z}$. Now since we have infinite choices for the top and the bottom, and there are even infinite fractions between any two integers[^3], you might think that $|\mathbb{Q}|>|\mathbb{N}|$. Since $\mathbb{N}\subset\mathbb{Q}$, we know that $|\mathbb{N}|\leq|\mathbb{Q}|$, but spoiling the results for you, we can show that

> **Theorem**: $|\mathbb{N}| = |\mathbb{Q}|$

The proof for this is fairly involved, but I will show you one way of listing out all the rational numbers that will in fact give you every single one. For the sake of simplicity we will only consider positive fractions, as negative fractions could just be inserted in between in the same way we did above for the integers. 

The idea is that we will list off all fractions sorted by the sum of their numerator and denominator, so start with all that sum to $1$, then $2$, then $3$, etc. etc. For each of these options, theres actually only a finite number of ways to represent this, so at some point we will reach every fraction (which would make the function that follows this order surjective). If we ever come across a fraction who is already in this list (for example $\frac{2}{4}$ when $\frac{1}{2}$ is there) we skip it, which will make our function also injective. The list will look like the following
$$
\frac{0}{1},\frac{1}{1},\frac{1}{2},\frac{2}{1},\frac{1}{3},\frac{3}{1},\ldots
$$
Notice we skip fractions such as $\frac{0}{2}$ as that appears in the start and we proceed to skip it.

This means that $\mathbb{Q}$ is also a countable even though it seems to be incredibly large! At this point you might be thinking *"Yeah but infinity is infinity, it's obvious they are all the same size"*, to which I say, just you wait for the next page

---

## Practice Problems

> **Theorem**: There are an infinite number of rational numbers $q$ such that $0<q<1$

> **Theorem**: The set of square numbers $\\{1,4,9,16,\ldots\\}$ is countable

[^1]: Unless you said otherwise but I won't blame you infinity is tough 😝

[^2]: Remember, this includes $0$

[^3]: Try proving this!