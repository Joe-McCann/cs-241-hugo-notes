---
title: Equivalence Classes
linktitle: Equivalence Classes
toc: true
type: book
date: 2023-06-29
draft: false
tags:
    - cs241
    - sets
    - relations
    - equivalence classes
    - quotient sets

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 270
---

## Why Duplicate Effort?

Suppose we have some equivalence relation $\approx$; note here $\approx$ does not mean "approximately equal", it just is a generic symbol for any equivalence relation. Since $\approx$ is an equivalence relation, then we know that it is **reflexive**, **symmetric**, and **transitive**.

Now say we know that $a\approx b$ and $b\approx c$ and $c\approx d$ and so on and so forth. By transitivity all of these items are related to each other, and more specifically we would say they are all *equivalent* under this relation. As such, we can really just group these items all together and when we want to work with any of them, we can just decide to work with one of them instead because they are all the same!

Given some item $x$ inside of our set $A$, we can take all the items that are equivalent to $x$ and put them into a set together. Such a set we will call an *equivalence class*.

> **Definition**: Let $R(\approx)\subseteq A\times A$ be an equivalence relation. For any $x\in A$, define the **equivalence class** of $x$ to be the set of all $y\in A$ that are equivalent to $x$, notated as $$[x]=\\{y | x\approx y\\}$$

Equivalence classes pop up *everywhere* in mathematics, and they form a central role in anything that defines its own version of equality. You wanna study number theory? Modular math is equivalence relation. Graph Theory or algorithms? Isomorphisms create equivalence classes of graphs. Linear algebra? Hope you like vector spaces. Because they are so fundemental with some very useful results, its a good idea to discuss them here.

Notice though that we chose any $x$ and then created the class $[x]$, since we are writing the class with $x$ in the brackets, we call $x$ the *representative* of the equivalence class. However, if some item $y\in [x]$ then there is no reason we couldn't have written the class as $[y]$ since all items are equivalent. Lets provide an example to show what we mean.

> **Example**: Define the equivalence classes for the relation $a\equiv b\mod 2$ where $a,b\in\mathbb{N}$. Remember that $a\equiv b\mod 2$ is true when $a,b$ have the same remainder when divided by $2$.

Lets start by listing out numbers and seeing what would have the same remainders. The first number of $\mathbb{N}$ is $0$, and there is no remainder when divided by $2$. So all the items equivalent to $0$ would be any other number with no remainder, which is any even number! Our first equivalence class is
$$
[0]=\\{0,2,4,6,8,\ldots\\}.
$$
Next, lets consider the next number not already in $[0]$ which is $1$. $1$ has a remainder of $1$ when divided by $2$, which is the same as all the odd numbers so
$$
[1]=\\{1,3,5,7,9,\ldots\\}.
$$
This covers all the numbers in $\mathbb{N}$, so all our equivalence classes are $[0],[1]$. In this case the representatives of our classes are $0,1$, but there is nothing stopping us from saying our classes are $[16],[31]$ or $[100],[3215]$. All of these classes are the same.

{{% callout warning %}}
If you are doing a proof involving a generic equivalence class $[x]$, you need to make sure that your proof works **no matter what representative is chosen**. For example using the example above, if you write a proof using $[0]$, but that proof doesn't work anymore if you use $[2]$ instead, your proof is **wrong** since $[0]=[2]$
{{% /callout %}}

---

### Properties of Equivalence Classes

Here we will prove two properties of equivalence classes that make them incredibly useful in all forms of mathematics, and then introduce some of the *coolest fucking notation*. But first lets show a relatively easy theorem to state and prove.

> **Theorem**: Let $\approx$ be an equivalence relation on $A$. $\forall x\in A$, there exists and equivalence class $[x]$ that has $x\in [x]$.

{{% callout info %}}
<details>
<summary>Proof</summary>
Since $\approx$ is an equivalence relation, this means that $\approx$ is reflexive and $x\approx x$ for every $x$. As such $x\in [x]$.
</br>
Q.E.D.
</details>
{{% /callout %}}

Straightforward enough to see that every item in $A$ is inside an equivalence class, even if that class contains only one item. Is it possible to have an item be in two different equivalence classes though?

> **Theorem**: Let $\approx$ be an equivalence relation on $A$. If $[x]\neq [y]$ then $[x]\cap [y]=\varnothing$. This means that different equivalence classes cannot share items.

{{% callout info %}}
<details>
<summary>Proof</summary>
For the sake of contradiction, assume that $[x]\neq [y]$ but $z\in [x]$ and $z\in [y]$. By definition, $z$ is equivalent to everything in $[x]$, and $z$ is also equivalent to everything in $[y]$. However, by transitivity, this would mean all the items in $[x]$ are equivalent to $[y]$, so all the items in $[x]$ are inside of $[y]$ and vice versa. This implies that $[x]=[y]$ which contradicts our initial assumption, and proves our theorem.
</br>
Q.E.D.
</details>
{{% /callout %}}

This is very interesting because it shows us that if we have an equivalence relation, every item in the set will be in **exactly** one equivalence class. In a sense we are dividing up our set into a bunch of distinct segments. This is called **partitioning** a set and actually it has the same purpose as the term in computer hardware! If you partition your hard drive between operating systems, you want every piece of memory to be used, and you *never* want your partitions overlapping! 

We can now take every equivalence class and add those to a set that has some *crazy* notation, because in essence we are dividing up our set, so lets show that!

> **Definition**: Let $\approx$ be an equivalence relation on $A$. The **Quotient Set** of $\approx$ on $A$ is the set of all equivalence classes of $\approx$. This is notated as $$A/\approx = \\{[x]|x\in A\\}=\\{[x_0],[x_1],[x_2]\ldots\\}$$

By definition from before we know that
$$
A = \bigcup_{[x_i]\in A/\approx} [x_i] 
$$
and
$$
[x_i]\cap [x_j] = \varnothing
$$
if $i\neq j$. This is a way of dividing up our set into equivalent pieces!

### An Example of the Quotient Set

Before we showed that all the equivalence classes of the relation $a\equiv b\mod 2$ were $[0]$ and $[1]$. If we add these into a set, we can create the quotient set of all the numbers equivalent $\mod 2$! Since $\mod 2$ is such annoyingly long notation though, it is often notated (in the shittiest way possible) as
$$
\mathbb{Z}/2\mathbb{Z} = \\{[0],[1]\\}
$$
Which is called the *integers modulo 2*[^1]. If we want to generalize this to $\mod n$ instead of specifically $2$, then our quotient set would be
$$
\mathbb{Z}/n\mathbb{Z} = \\{[0],[1],[2],\ldots,[n-1]\\}.
$$

We will discuss these sets *a lot* in the future, and often times people drop the brackets in order to *really* confuse everyone, to say
$$
\mathbb{Z}/n\mathbb{Z} = \\{0,1,2,\ldots, n-1\\},
$$
but in reality, each single digit represents an equivalence class thats also worked with as a number? Oh boy this shit gets crazy, look forward to number theory!

---

## Example Problems

> **Example**: Let $P=\\{P_0,P_1,\ldots\\}$ be a partition of some set $A$, where every item of $A$ is inside exactly one member of $P$. Show that you can find an equivalence relation $\approx$ such that $P=A/\approx$. 

[^1]: We will later define how to deal with negative numbers but just roll with it for now.
