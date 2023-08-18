---
title: Primes and Divisibility
linktitle: Primes and Divisibility
toc: true
type: book
date: 2023-08-15
draft: false
tags:
    - cs241
    - number theory
    - primes
    - integers
    - divisibility

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 205
---

## Integers Made up of Small Pieces

Think back to elementary school, a time when things were simple and you had recess. Despite the most difficult math being times tables, we still said math classes were hard 💀 oh how times have changed.

When learning about division, sometimes when you divided two numbers it wouldn't go in cleanly and there would be a *remainder* left over. For example, dividing $7$ by $3$ nets a remainder of $1$, and we would say that this meant $7$ was not *divisible* by $3$. But now we are big kids and the days of loose math terminology is over, what does it really mean for a number to be divisible by another?

> **Definition**: Let $a,b\in\mathbb{Z}$. $a$ is **divisible** by $b$ if we can find a value of $k\in\mathbb{Z}$ such that $a=k\cdot b$. This is notated as $b|a$ or "b **divides** a". If $b$ divides $a$ then $b$ is a **factor** of $a$.

Clearly every number is a factor of itself, and has $1$ as a factor, which we can prove.

> **Theorem**: Let $a\in\mathbb{Z}$. $1|a$ and $a|a$
{{% callout info %}}
<details>
<summary>Proof</summary>
Since every number multiplied by $1$ is itself, we can know that $a=1a$ which by definition means that both $1,a$ are factors of $a$.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

However most numbers have other factors too, for example $2|4$, $9|45$, or $25|100$. As such, numbers that *only* have $1$ and itself as factors are special, and we call them *prime*.

> **Definition**: An integer $p>1$ such that $p$ is only divisible by $1$ and $p$ is a **prime** number. A number that is not prime is called **composite**

You could also rephrase this as: $p>1$ is prime iff for all $x>0$, if $x|p$, then $x=1$ or $x=p$. 

Note the requirement $p>1$, which restricts $1$ to not be a prime number. You might be wondering *"$1$ feels like it should be a prime number so why isn't it?"* The answer is quite bland in that, if $1$ was a prime number it would make so many theorems way more annoying to write out and deal with, and including $1$ as a prime number adds pretty much not benefit whatsoever. As such we just say $1$ is not prime out of convenience lmao. 

The sequence of prime numbers is an interesting one
$$
2,3,5,7,11,13,17,\ldots
$$
as there is seemingly no pattern at all. Many, many, mathematicians have spent many hours trying to figure out ways to express the prime numbers in a nice clear formula or expression, but none exists (and probably won't ever). We will talk about primes more later, but lets discuss a bit more on divisibility.

---

### Properties of Divisibility

Lets look at the relation[^1] $b|a$ when $a,b\in\mathbb{Z}$. We can begin with properties of relations

> **Theorem**: For all $a\in\mathbb{Z}$, $a|a$. This means divisibility is reflexive.

This is actually already proven as before, so we don't need to do it again.

> **Theorem**: For all $a,b,c\in\mathbb{Z}$, if $a|b$ and $b|c$, then $a|c$. This means divisibility is transitive.
{{% callout info %}}
<details>
<summary>Proof</summary>
We know that $a|b$ so $b = k_1a$. We also know that $b|c$ so $c=k_2b$. Combining these we get $c=k_1k_2a=ka$ if $k=k_1k_2$ which means that $a|c$. 
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

Giving an example of this, if we know that $7|35$ and that $35|105$, then that means that $7|105$, which it does!

> **Theorem**: For all $a,b\in\mathbb{Z}$, if $a|b$ and $b|a$, then $a=b$. This means divisibility is anti-symmetric.

I'm evil and will provide this as a homework problem because its actually kinda fun haha.

Now lets move onto two theorems that are actually fairly interesting involving divisibility when you are adding numbers together. The first is fairly uninteresting.

> **Theorem**: If $a|b$ and $a|c$ then $a|b+c$.
{{% callout info %}}
<details>
<summary>Proof</summary>
We know that $a|b$ so $b = k_1a$. We also know that $a|c$ so $c=k_2a$. Combining these we get $b+c=k_1a+k_2a=(k_1+k_2)a$ which means that $a|b+c$. 
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

This isn't very surprising as we can see that $5|25$ and $5|135$ so $5|25+135$ just by pulling out a factor of $5$ with the distributive law. A more interesting version of this theorem though comes as the following.
> **Theorem**: If $a|b$ and $a\nmid c$ then $a\nmid b+c$.
{{% callout info %}}
<details>
<summary>Proof</summary>
To prove this we will actually perform a proof by contradiction. We know that $a|b$ so $b=k_1a$, and suppose that $a|b+c$ so $b+c=k_2a$. Plugging in for $b$ we can see that 
$$
k_1a+c=k_2a\implies c=k_2a-k_1a=(k_2-k_1)a=ka.
$$
This means that $a|c$ which contradicts our claim, meaning that $a\nmid b+c$.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

So if we know that $2|8$ and $2\nmid 13$, then we know that $2\nmid 8+13$. This will actually prove to be fairly valuable for some later proofs!

### Brief Aside: Even and Odds

For fun, let us define what it means for a number to be even versus odd. We know that an even number is divisible by $2$ and odd numbers are not, so we can say

> **Definition**: An integer $n$ is **even** iff there exists a $k\geq 0$ such that $n=2k$. An integer $n$ is **odd** iff there exists a $k\geq 0$ such that $n=2k+1$.

We can use these definitions for some cute little practice problems

> **Corollary**: The sum of two even numbers is even.

I say this is a corollary because literally by the theorem above, if $2|n_1$ and $2|n_2$ then $2|n_1+n_2$, but lets go through the full effort just for fun.

{{% callout info %}}
<details>
<summary>Proof</summary>
Since $n_1, n_2$ are even we know $n_1=2k_1$ and $n_2=2k_2$. Then we have
$$
n = n_1+n_2 = 2k_1+2k_2 = 2(k_1+k_2) = 2k
$$
which is even.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

I have more cute little examples in the practice problem section

---

## Getting Started with Primes

Prime numbers are incredibly captivating to mathematicians because of a super, super important result that we will get to later down the line, however before we get to that, lets show some interesting properties of prime numbers right now.

### Simple (and some not simple) Properties of Primes

Modern number theory is littered with unsolved problems involving prime numbers Here we will show some of the properties of primes that we do in fact know. First lets begin with the question of how many prime numbers are there? People keep finding very, very large prime numbers so is there a limit at some point? Well this was originally proven in Euclids book *Elements*, written in $300$ B.C., and it was proven to be the following

> **Euclid's Second Theorem**: There are infinite prime numbers.
{{% callout info %}}
<details>
<summary>Proof</summary>
We will prove this by contradiction, first begin by assuming there are a finite $n$ number of primes. Let's list them all out
$$
p_1,p_2,p_3,\ldots, p_n.
$$
We now will consider the number that is the product of every one of our primes, so
$$
P=p_1p_2\ldots p_n.
$$
By definition, $P$ is divisible by every prime so for all $j$, $p_j|P$. Notice though that since $2$ is the smallest prime, for every $j$, $p_j\nmid 1$ which means that by the theorem we proved above that for all $j$
$$
p_j\nmid P+1.
$$
Since $P+1$ is not divisible by any prime in our list, and we said our list contains all the primes, that means that $P+1$ must be prime! But $P+1$ is not in our list which contradicts our assumption and proves our claim.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

This theorem has been proven *many, many* times in a variety of ways, and is the basis for modern day number theory[^3].

A lot of students often mistake this proof to mean that if you take the product of the first few primes and add $1$ that you'll get a prime number. This is not always true, as in the proof we were working in a world in which there were finite primes (which there aren't), however some of them are and these are called [primorial primes](https://en.wikipedia.org/wiki/Primorial_prime) (the product of the first $n$ primes is called the $n^{\text{th}}$ primorial). This isn't that important but wanted to address the common misconception.

Ok, so if there are infinite primes, can we at least say how far out we need to go before we see the next one? I mean, if we have some number $p$ that we know is prime, can we guarantee that the next prime is at most $10^6$ away (for example)? The answer here is also no.

> **Theorem**: For every integer $N>0$, there exist consecutive primes $p_n, p_{n+1}$ such that $p_{n+1}-p_n\geq N$.
{{% callout info %}}
<details>
<summary>Proof</summary>
In order to prove this, we will find a sequence of not prime numbers that is $N$ long, such that if you take the first prime before this sequence, and the first prime after this sequence will have a gap between them bigger than $N$.

Consider the number $(N+1)!=1\cdot 2\cdot 3\cdot \ldots \cdot (N+1)$. We know by definition that for every $2\leq k\leq N+1$ that $k|(N+1)!$. As such by divisibility rules we know
$$
\begin{align}
2&|(N+1)!+2 \\
3&|(N+1)!+3 \\
4&|(N+1)!+4 \\
&\vdots \\
N+1&|(N+1)!+(N+1) \\
\end{align}
$$
since this sequence of $N$ numbers are all composite, we know that the gap between the primes before and after this sequence must be $\geq N$
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}
This is quite strange, as we can make $N$ as large as we'd like and always find a corresponding gap! $(N+1)$ is quite big, however this is an overestimate as you can show that a gap occurs at the $N^{\text{th}}$ primorial (try it yourself to prove!). Just to give an example, $6!=720$ so
$$
2|722, 3|723, 4|724, 5|725, 6|726
$$
which means that we know we have a prime gap of at least $5$ here, in fact $719,727$ are primes which actually have a gap of $8$ between them. The first actual prime gap of $5$ occurs between $23,29$ which means we overestimated a bit LOL. The primorial method predicts $32,33,34,35,36$ which is much closer to the true solution

Ok so there is no constant bound on prime number gaps, but what if the gap between primes grows over time, can we get an asymptotic value there 👀? In the late 1800s a really interesting theorem was proven

> **Bertrand's Theorem**: For every integer $n>1$, there exists a prime number $p$ such that $n < p < 2n$.

The theorem involves some more high powered number theory, so I am not going to put it here, but this means that we actually do have a relatively good window for which we know where the next prime number will appear! So given some number for example $1023$, we know that there must be a prime number $p$ where $1023 < p < 2046$. This also means that we know that the gap between two prime numbers must be $\mathcal{O}^T(n)$, and its actually conjectured to be way way better than that.

---

## Practice Problems

> **Theorem**: For all $a,b,c\in\mathbb{Z}$, if $a|b$, then $a|bc$

> **Theorem**: The sum of an odd and an even number is odd

> **Theorem**: The product of two odd numbers is odd

> **Theorem**: Proof that there are infinite primes $p$ such that there exists a $k\in\mathbb{Z}_+$ such that $p=4k+3$

> **Theorem**: Prove that for every value $n\geq 1$, there exists a prime $p$ such that $n^2 < p < (n+1)^2

[^1]: You thought we were done with relations hahaha.
[^3]: Kinda, theres another super important theorem too.
