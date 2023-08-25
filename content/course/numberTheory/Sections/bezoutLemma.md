---
title: Bezout's Lemma and the Fundemental Theorem of Arithmetic
linktitle: Bezout's Lemma and FTA
toc: true
type: book
date: 2023-08-22
draft: false
tags:
    - cs241
    - number theory
    - integers
    - gcd
    - complexity
    - diophantine equations

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 215
---

## Unlimited Power ⛈️

Now that we have the machinery[^1] of the $\gcd$ and general divisibility, we can touch on a theorem that will unlock us the ability to prove a ton of problems that otherwise would have been super freaking annoying to solve without it. Note that much of the information on this page was referenced from [brilliant.org](https://brilliant.org/wiki/bezouts-identity/) which is a great resource for such information.

> **Bezout's Lemma**: For all values of $a,b,k\in\mathbb{Z}$, the equation $ax + by = k$ where $x,y$ are integer variables has a solution iff $\gcd(a,b) | k$

This solution is actually quite insane, as we can see whether or not there will be solutions to equations by observing the Euclidean Algorithm.

$$
5x+3y = 1,
$$
$\gcd(5,3)=1|1$ so we know a solution exists.
$$
2x+4y = 15 
$$
$\gcd(2,4)=2\nmid 15$ which means no solution exists. This will have many applications, but let us first provide the proof.

First, the forward direction that `solution exists` implies $\gcd(a,b)|k$ is quite easy by contrapositive, as if $\gcd(a,b)\nmid k$ then if you divided both sides by the $\gcd$ then $\frac{k}{\gcd(a,b)}$ would not be an integer and the equation would not be solvable with integers. Now for the more challenging direction

In order to prove that $\gcd(a,b)|k$ implies `solution exists` we will actually provide an algorithm to find the solution. This algorithm is called the *Extended Euclidean Algorithm* and actually is just the Euclidean Algorithm, but holding onto some extra variables.

The idea of the algorithm is that we are going to find values $(x_0, y_0), (x_1, y_1), \ldots$ such that at every step we get
$$
r_j = ax_j + bx_j
$$
where each $r_j$ is a remainder in the Euclidean Algorithm. If we can eventually get to the final step, we will be able to solve the equation for the $\gcd(a,b)$. We start with
$$
\begin{align}
x_0=1, y_0=0, r_0=a \\\\
x_1=0, y_1=1, r_1=b.
\end{align}
$$
This should make sense as then $ax_0+by_0=a$ and $ax_1+by_1=b$ which is correct. From here we proceed with the Euclidean algorithm as well as two additional steps. Let
$$
a = qb + r_2 \implies r_2 = a - q_1b
$$
and in general we would have
$$
r_{n+1} = r_{n-1}-q_nr_n
$$
with $q_n = \lfloor{\frac{r_{n-1}}{r_n}}\rfloor$. Taking this value of $q_n$ we will say that
$$
\begin{align}
x_{n+1} &= x_{n-1}-q_nx_n \\\\
y_{n+1} &= y_{n-1}-q_ny_n.
\end{align}
$$

We can show that this algorithm will always produce valid steps as
$$
\begin{align}
&ax_{n+1}+by_{n+1} \\\\
=&a(x_{n-1}-q_nx_n)+b(y_{n-1}-q_ny_n) \\\\
=&(ax_{n-1}+by_{n-1})-q_n(ax_n+by_n) \\\\
=&r_{n-1}-qnr_n \\\\
=&r_{n+1}.
\end{align}
$$
Therefore, as long as we eventually reach the $\gcd(a,b)$ we will find a value of $x,y$ that solves the equation; since the Euclidean algorithm guarantees the $\gcd$, this is done.

If $k=\gcd(a,b)$ then we are done, however if $k=c\gcd(a,b)$, then find the final solutions of $x,y$ that solve for the $\gcd$, and then multiply both sides of the equation by $c$. This proves our claim

**QED**.

*Holy shit* that was a lot. When I taught this for the first time to my students, literally $0$ people understood what was going on, so it might be worthwhile to go through an example. Notice that this algorithm is also $\mathcal{O}(\log n)$ as its just the Euclidean algorithm but with some additional steps each iteration.

---

### Walking through the Extended Euclidean Algorithm

> **Example**: Find values $x,y$ that satisfy
$$
134x - 120y = 12
$$

We will, by hand, compute the extended Euclidean Algorithm to solve this problem. We start with
$$
\begin{align}
r_0= 134,x_0=1,y_0=0 &\implies 134(1)-120(0)=134 \\\\
r_1=-120,x_1=0,y_1=1 &\implies 134(0)-120(1)=-120
\end{align}
$$
We now proceed with the next step. $134 = (-120)(-1) + 14$ which means that $q_1=-1$ and $r_2=14$. Also 
$$
\begin{align}
x_2 &= x_0-q_1x_1 = (1)-(-1)(0) = 1 \\\\
y_2 &= y_0-q_1y_1 = (0)-(-1)(1) = 1
\end{align}
$$
which gives us that $134(1)-120(1)=14$ which is correct. We now go to the next step to see $-120=14(-8)-8$ so $q_2=(-8)$ and $r_3=-8$.
$$
\begin{align}
x_3 &= x_1-q_2x_2 = (0)-(-8)(1) = 8 \\\\
y_3 &= y_1-q_2y_2 = (1)-(-8)(1) = 9
\end{align}
$$
and again we get $134(8)-120(9)=-8$ which is true. We still have not hit a remainder of $0$ yet so we continue. $14=(-8)(-1)+6$ meaning $q_3=-1$ and $r_4=6$.
$$
\begin{align}
x_4 &= x_2-q_3x_3 = (1)-(-1)(8) = 9 \\\\
y_4 &= y_2-q_3y_3 = (1)-(-1)(9) = 10
\end{align}
$$
and to confirm we find that $134(9)-120(10)=6$. Continuing we get that $-8=(6)(-1)-2$ with $q_4=-1$ and $r_5=-2$.
$$
\begin{align}
x_5 &= x_3-q_4x_4 = (8)-(-1)(9) = 17 \\\\
y_5 &= y_3-q_4y_4 = (9)-(-1)(10) = 19
\end{align}
$$
shockingly we find that $134(17)-120(19)=-2$. When we do $6=(-2)(-3)$ we find a remainder of $0$ which means our algorithm is done. While we could leave it like this, we generally want to present our $\gcd$ positive, so we would multiply both sides by $-1$ to get
$$
\begin{align}
(-1)\left(134(17)-120(19)\right)&=(-1)(-2) \\\\
134(-17)-120(-19)&=2 
\end{align}
$$
Now since we want to solve for when the equation equals $12$, we can multiply both sides by $\frac{12}{\gcd(134,120)}=6$ to get that
$$
\begin{align}
(6)\left(134(-17)-120(-19)\right)&=(6)(2) \\\\
134(-102)-120(-114)&=12,
\end{align}
$$
which plugging into a calculator we find is correct! So a valid solution is $x=-102$ and $y=-114$. If we wanted smaller positive numbers, we could add $120$ and $134$ to our solutions which would cancel out but make things easier, so
$$
134(-102+120)-120(-114+134)=134(-102)-120(-114)+134\cdot 120-134\cdot 120 = 12
$$
so we get $-102+120=18$ and $-114+134=20$
$$
134(18)-120(20)=12
$$

so $x=18$ and $y=20$ is another valid solution.

## The Fundemental Theorem of Arithmetic

Now that we have access to Bezout's Lemma, we now have the tools that we need to easily prove the Fundemental Theorem of Arithmetic, which forms the basis of number theory and why we really care so much about prime numbers.

*But first* before we actually explain what that is, we need to prove something called Euclid's Lemma first. Funny enough this little Lemma is why we need Bezout's Lemma, if we just took it by assumption then we could have skipped all this effort LOL. Crazier, Euclid proved this shit using fucking Geometry what a psychopath 🔪🩸

> **Euclid's Lemma**: Let $p$ be a prime number. If $p|ab$ then $p|a$ or $p|b$.
{{% callout info %}}
<details>
<summary>Proof</summary>
In order to prove this, we will show that if $p\nmid a$ then its required that $p|b$ which is sufficient to prove, as if $p|a$ then we're done. We know that $p|ab$ which means that$$
ab = kp.$$
Since $p|a$ and $p$ is prime, then $\gcd(a,p)=1$ since the only factor of $p$ is $p$ and $1$. As such by Bezout's Lemma we know that $$
px + ay = 1$$ has a solution. Multiply both sides by $b$ to get that $$
pbx + aby = b.$$ Notice before that we said $ab=kp$ so we can substitute to get that $$
pbx + kpy = b \implies p(bx+ky)=b$$ which means that $p|b$ as we know a solution exists and otherwise $\frac{b}{p}$ would not be an integer. This proves our claim. 
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

With this result we are now ready to tackle the Fundemental Theorem.

> **Fundemental Theorem of Arithmetic**: For every integer $n>1$, there exists a unique product of primes
$$
n = p_1^k_1p_2^k\ldots p_mk^m.
$$
In other words, every number has a prime factorization.

This is how we know that every number can be broken up into prime factors! Now like many theorems, this one has two pieces. The first is showing that a factorization always exists, and the second is showing that every number has only one factorization[^2]. We will do these two proofs seperately; existence is easy, uniqueness is hard (kinda).

---

## Practice Problems

> **Example**: Find a value $x,y$ such that $76x+12y = 16$

> **Theorem**: Let $\gcd(n,a)=1$ and $n|ab$. Then $n|b$

[^1]: Term mathematicians use to describe the idea that we have clean ways of doing and describing things
[^2]: Shuffling the terms is considered the same product