---
title: Fermat and Euler Teamup
linktitle: FlT and Totients
toc: true
type: book
date: 2023-08-29
draft: false
tags:
    - cs241
    - number theory
    - integers
    - modular arithmetic
    - residues
    - totient
    - flt

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 230
---

## Euler's Totient Function

In this page we will discuss some interesting properties, and get us to the point that we need in order to understand RSA Encryption from scratch.

Now that we have a distinction between residue classes and prime residue classes, we can see that some numbers seem to be "less prime" than other numbers. I mean $5$ had everything except $0$ be relatively prime to it, whereas $6$ *only* had $1,5$ relatively prime to it, and $14$ was kinda in the middle.

What would be valuable is a function that we could have to give us these measures of how prime or not prime we are. This is the idea behind Euler's Totient Function, notated as $\phi(n)$. In written form, we would say that this function counts how many numbers $k$ such that $0\leq k < n$ that are relatively prime to $n$. So for example
$$
\begin{align}
\phi(5) &= 4 \\\\
\phi(6) &= 2 \\\\
\phi(14) &= 5
\end{align}
$$
using our examples from prior. Notice that is is also literally just taking the cardinality of the set $R_n$. 

> **Euler's Totient Function**: Let $\phi(n)$ be the function that returns the number of values $0\leq k < n$ such that $\gcd(k,n)=1$. Technically speaking we can define $\phi(n)$ as
$$
\begin{align}
\phi:\mathbb{N}&\rightarrow\mathbb{Z}_+ \\\\
\phi(n) &= |R_n|
\end{align}
$$
where $|R_n|$ represents the cardinality.

Now one thing to notice is that for values of $p$ that are prime, $\phi(p)=p-1$, which comes from the fact that prime numbers are divisible by nothing except $1$ and themselves, so the only number they are not relatively prime to is $0$ (remember $1$ is relatively prime to everything).

Before moving on to the main theorem(s) of this page, I will state a property of $\phi(n)$ that we will need later down the line

> **Theorem**: If $\gcd(m,n)=1$ then $\phi(m\cdot n)=\phi(m)\cdot\phi(n)$

The proof of this I think is a bit involved so for now I'mma skip it, however I will get to it eventually.

---

## A Theorem of Fermat and Euler[^1]

Going back to our favorite little goofball Fermat, he is responsible for a theorem that is the basis for a lot of divisibility testing and even a (fake) primality test. The history goes that he told this theorem to one of his friends as another thing that looked true but never proved it -- although there was an obvious special case that proves it false he apparently ignored but we're goofy here who cares.

The original version that he claimed was

> **Fermat's Little Theorem**: Let $p$ be prime and $a$ is any integer such that $p\nmid a$, then
$$
a^{p-1} \equiv 1 \mod p
$$
although many people use the following version so they don't need to worry about the condition $p\nmid a$

> **Fermat's Little Theorem**: Let $p$ be prime and $a$ any integer, then
$$
a^p \equiv a \mod p.
$$

You can see that these two theorems are literally the same exact thing multiplying both sides by $a$. Now this is called Fermat's Little Theorem, because its important enough to get a name, but also just calling it Fermat's Theorem would get it confused with Fermat's Last Theorem. From a funny coincidence, those have the same initials, so the Last Theorem is abbreviated FLT, and the Little Theorem is FlT (with a lowercase l 🤣).

We can have some silly examples that are easily proven using this

> **Example**: For all values of $n$, $42|n^7-n$

$42=2\cdot 3\cdot 7$ so if we divide by all those factors then we are done. We know $n^7-n=n(n^6-1)$ and by FlT we know if $7\nmid n$ then
$$
\begin{align}
&n^6 \equiv 1\mod 7  \\\\
\implies &n^6-1 \equiv 0\mod 7 \\\\
\implies &7 | n^6-1.
\end{align}
$$
Since we have $n(n^6-1)$, if $7|n$ then we are still good by that first factor of $n$. We also know that by FlT that if $3\nmid n$ that
$$
\begin{align}
&n^2 \equiv 1 \mod 3 \\\\
\implies &\left(n^2\right)^3 \equiv 1^3\mod 3 \\\\
\implies &n^6 \equiv 1\mod 3 \\\\
\implies &3 | n^6-1.
\end{align}
$$
Similarly we know that if $3|n$ then we still have that first factor of $3$. Finally if $2\nmid n$ then
$$
\begin{align}
&n \equiv 1 \mod 2 \\\\
\implies &n^6 \equiv 1 \mod 2 \\\\
\implies &2 | n^6-1
\end{align}
$$
and if $2|n$ then were good. To conclude, we are sure that $2$ divides either $n$ or $n^6-1$, $3$ divides either $n$ or $n^6-1$, and the same for $7$. This means $42|n^7-n$.

I will admit, this is a longer version of what is needed, but I liked spelling it out.

I also didn't prove this theorem, because it turns out a more general version of the theorem was proven by Euler later, and that is the version that we will prove

> **Fermat-Euler Theorem**: Let $\gcd(a,n)=1$, then
$$
a^{\phi(n)}\equiv 1\mod n
$$

Even though this doesn't handle the case where $n|a$, which is honestly not that deep, this theorem is super super general. We can use it to say if $n=16$, $\phi(16)=8$, $a=21$ that
$$
21^8\equiv 1 \mod 16
$$

A clever computer scientist might be able to use this to their advantage to speed up computations. The real usecase power of this theorem though comes with its usage in RSA encryption which is the standard encryption scheme for the entire internet. Before we can do that though, we need to prove the theorem.

The proof is shockingly simple.

*Proof:*

For any value of $n$, let
$$
R_n = \left\\{[x_1],[x_2],\ldots,[x_{\phi(n)}]\right\\},
$$
be our complete set of prime residues $\mod n$. If $\gcd(a,n)=1$, from before we know that we can also say that
$$
R_n = \left\\{[ax_1],[ax_2],\ldots,[ax_{\phi(n)}]\right\\}.
$$
Since these are the same set, we must know that
$$
\prod_{i=1}^{\phi(n)}ax_i \equiv \prod_{i=1}^{\phi(n)}x_i \mod n.
$$
We are dealing with the equivalence class representatives here, but that is ok because they are all equivalent $\mod n$.
Continuing we see that
$$
a^{\phi(n)}\prod_{i=1}^{\phi(n)}x_i \equiv \prod_{i=1}^{\phi(n)}x_i \mod n,
$$
and we can cancel out the product term from each side since every $x_i$ is prime to $n$. This gives us the desired result that
$$
a^{\phi(n)}\equiv 1 \mod n
$$

**Q.E.D.**

---

## Practice Problems

> **Lemma**: If $p$ is prime, then $\phi(p)=p-1$

[^1]: In case you missed it on the intro page I *heavily* rely on **An Introduction to the Theory of Numbers** for this material. Literally this heading is from there and its so fucking slick I couldn't think of anything better.