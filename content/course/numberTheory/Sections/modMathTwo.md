---
title: Modular Arithmetic Part 2
linktitle: Mod Math Part 2
toc: true
type: book
date: 2023-08-29
draft: false
tags:
    - cs241
    - number theory
    - integers
    - modular arithmetic
    - relations
    - residues
    - totient

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 225
---

## Continuing Where We Left Off

On this page we will discuss some theorems and topics that are slightly more advanced and would likely scare 👻 anyone away.

Remember that equivalence $\mod n$ is literally an equivalence relation, that means we can actually create equivalence classes of items that are all equivalent to each other. As a mini refresher, lets do the following example

> **Example**: Write all equivalence classes of the integers modulo $5$, which is the quotient set of the integers $\mod 5$

Lets write out the quotient set starting with $[0]$ which is all the items equivilent to $0\mod n$. This is
$$
[0]=\\{0, 5, -5, 10, -10, 15, \ldots\\}
$$
and we can do the same thing with all the other integers
$$
\begin{align}
[1]&=\\{1, 6, -4, 11, -9, 16, \ldots\\} \\\\
[2]&=\\{2, 7, -3, 12, -8, 17, \ldots\\} \\\\
[3]&=\\{3, 8, -2, 13, -7, 18, \ldots\\} \\\\
[4]&=\\{4, 9, -1, 14, -6, 19, \ldots\\}.
\end{align}
$$

This is all possible equivalence classes, which gives us that our quotient set is $\left\\{[0],[1],[2],[3],[4]\right\\}$. Notice that, even though we could have picked any item of the set, for all the representatives of our equivalence classes, I selected the smallest non-negative item. In order to explain why this is meaningful lets define some terminology. Normally when we say that
$$
26\equiv 1\mod 5
$$
we think of $1$ as the remainder from division, which is true. However, there is nothing saying that the remainder *needs* to be smaller than $5$ similarly we have
$$
26\equiv 11\mod 5\implies 26 = 3\cdot 5 + 11
$$
whereas in this case $11$ is still a remainder, just not the smallest possible remainder that we can have. In fact there is a term for this remainder[^1]

> **Definition**: Given some value $k$, if we have that
$$
k\equiv a\mod n
$$
then we say that $a$ is a **residue** of $k$ $\mod n$. If $0\leq a < n$ then we say that $a$ is the **least residue** of $k$.

So in our prior example when $k=26$ and $n=5$ we have that $11$ is a residue and $1$ is the least residue. In fact, $26$ is actually a residue of itself! Notice that all residues are equivalent and are in the same equivalence class, and similarly we have that items in different equivalence classes are not equal; this all follows from equivalence class properties and partitions.

> **Definition**: The quotient set of all equivalence classes $\mod n$ is called a **complete system of incongruent residues**, and is notated as
$$
\mathbb{Z}/n\mathbb{Z} = \left\\{[0],[1],\ldots, [n-1]\right\\}.
$$
Often people will leave out the square brackets and only include the least residue, and in the notation often leave out the first $\mathbb{Z}$ writing
$$
n\mathbb{Z}= \left\\{0,1,\ldots, n-1\right\\}.
$$

So from our previous example we would have that $5\mathbb{Z}=\\{0,1,2,3,4\\}$ which is a brief way to write the subsets of integers.

This might seem like a pretty theoretical way of looking at things, but surprisingly modular arithmetic proofs can include the usage of sets of residue classes. Lets prove a small lemma that will provide us some help later.

> **Lemma**: If $[x], [y]$ are two distinct residue classes $\mod n$, and $\gcd(k,n)=1$, then $\cdot[kx]\neq [ky]$

Before we prove this what is this saying. We are saying that if you have a number that is coprime to $n$ and then multiply the items of two *different* equivalence classes by said number $k$, then those two classes will still be different to each other (although they may or may not be the same class as before). We can see in our $5\mathbb{Z}$ example that $[0]\neq [4]$. Since $\gcd(5,7)=1$ we know that $[7\cdot 0]\neq [7\cdot 4]$ which is $[0]\neq [3]$[^2]. Now let us prove this

{{% callout info %}}
<details>
<summary>Proof</summary>
Let $a\in[x]$ and $b\in[y]$. We know that $$a\not\equiv b\mod n\implies n\nmid a-b.$$
As such since $\gcd(n,k)=1$ we know that 
$$n\nmid k(a-b)\implies n\nmid ka-kb\implies ka\not\equiv kb\mod n$$

Since $ka\not\equiv kb$ they cannot be in the same equivalence class which means the two classes must be distinct.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

> **Corollary**: If $\gcd(k,n)=1$ and
$$
\mathbb{Z}/n\mathbb{Z} = \left\\{[a_1],[a_2],\ldots,[a_n]\right\\},
$$
where $a_j$ may not necessarily be the least residue, then 
$$
\mathbb{Z}/n\mathbb{Z} = \left\\{[ka_1],[ka_2],\ldots,[ka_n]\right\\}.
$$

This theorem is actually insanely easy to prove based on the previous lemma we have. However the hardest part is saying what this theorem is actually doing. Pretty much just take all the equivalence classes of the quotient set, and if you multiply by a number prime to $n$ then you will still have all items of your quotient set. For $n=5$ and $k=7$ then
$$
\begin{align}
\mathbb{Z}/5\mathbb{Z}&=\left\\{[0],[1],[2],[3],[4]\right\\} \\\\
\mathbb{Z}/5\mathbb{Z}&=\left\\{[7\cdot0],[7\cdot1],[7\cdot2],[7\cdot3],[7\cdot4]\right\\} \\\\
\mathbb{Z}/5\mathbb{Z}&=\left\\{[0],[7],[14],[21],[28]\right\\} \\\\
\mathbb{Z}/5\mathbb{Z}&=\left\\{[0],[2],[4],[1],[3]\right\\}.
\end{align}
$$

### Prime Residues

---

## Practice Problems

> **Example**: Write out $4$ residues of $29\mod 13$, including the least residue

[^1]: Of course it can't just be remainder that would be too easy obviously
[^2]: Note that we are doing math on the representative for the sake of brevity, but this can be proven rigorously using mod math
