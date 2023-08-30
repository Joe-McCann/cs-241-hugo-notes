---
title: Greatest Common Divisor and Division
linktitle: Division and the GCD
toc: true
type: book
date: 2023-08-15
draft: false
tags:
    - cs241
    - number theory
    - primes
    - integers
    - division
    - gcd
    - complexity

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 210
---

## Remainders and Division

Before we get to a topic that is incredibly important in number theory, lets talk about division of integers and the different pieces that you can get when you divide numbers up into integer parts. In our previous page, we talked all about divisibility, but what happens if we want to divide numbers that aren't divisible by each other? Like what is $\frac{25}{7}$? We don't have decimals in the integers? Does the universe just explode?

No, what we can do is what you did in grade school and split the division into a *quotient* and a *remainder*. As you might remember seeing it
$$
\frac{25}{7} = 3 + \frac{4}{7},
$$
where you would say that $3$ is the quotient and there is a remainder of $4$. Theres still no decimals in the integers, so lets multiply both sides by $7$ to get
$$
25 = 3\cdot 7 + 4.
$$

In general we want to be able to do this for any time we divide two numbers, is that possible? It turns out the answer is yes!

> **Euclidean Division Theorem**: Let $a,b$ be integers and $b\neq 0$. There exists a unique quotient $q$ and remainder $r$ such that
$$
a = qb + r
$$
and $0 \leq r < |b|$.

Damn, y'all probably are already sick of existence and uniqueness theorems huh? Lets first prove existence. We will be proving existence for $a,b > 0$, however this is sufficient to prove the claim as explained [here](https://en.wikipedia.org/wiki/Euclidean_division#Existence)[^1]
{{% callout info %}}
<details>
<summary>Existence Proof</summary>
We will show an algorithm that when repeated will provide us with a solution. First, begin by setting $q_0 = 0$ and $r_0=a$. By definition we know that
$$
a = q_0b + r_0.
$$
If $r_0 < b$ then we are done. If $r_0 \geq b$, then we will set $q_1 = q_0+1 = 1$ and $r_1 = r_0 - b$. You can plug in and see that
$$
a = q_1b + r_1.
$$
If $r_1 < b$ then we are done. If not then we repeat the process with $q_{n+1} = q_n + 1$, and $r_{n+1}=r_n-b$. With this method we will eventually find values of $q,r$ that satisfy $0\leq r < b$.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

Fun fact, this provides an algorithm for division! Just keep subtracting and keeping count until you get to a remainder $0\leq r < b$. This isn't an efficient algorithm as it runs in $\Theta\left(\frac{a}{b}\right)$, but hey its something. Now lets prove the second part
{{% callout info %}}
<details>
<summary>Uniqueness Proof</summary>
Let us assume that there is a value of $a,b$ for which the division has more than one value of $q,r$. So we have$$
\begin{align}
a &= q_1b + r_1 \\\\
a &= q_2b + r_2 
\end{align}$$
but $q_1\neq q_2$ or $r_1 \neq r_2$. Setting these equations equal to each other we get that
$$
q_1b + r_1 = q_2b + r_2 \implies b(q_1-q_2) = r_2-r_1.
$$
This means that $b|r_2-r_1$ and since both $0\leq r_1,r_2 < |b|$, this means that $r_2-r_1=0$. We then can plug this in to show 
$$
b(q_1-q_2)=0\implies q_1-q_2=0$$
since $b\neq 0$. As such we know that $q_1=q_2$ and $r_1=r_2$ contradicting our assumption and proving uniqueness.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

---

## The Greatest Common Divisor

Divisibility is cool, but sadly in terms of relations its only a *partial* ordering because sometimes you just have numbers that don't divide each other. For example $24\nmid 16$ and $16\nmid 24$. However, it might be cool to define some way to measure the divisibility of the two numbers, even when they aren't actually divisible by each other.

In fact, for our previous example, both $16,24$ are divisible by $8$, whereas a pair of numbers like $5,7$ seemingly have nothing in common. This motivates the idea we have for the following definition

> **Definition**: The **greatest common divisor** of two numbers $a,b$ is the largest integer $n$ such that $n|a$ and $n|b$, notated as $\gcd(a,b)=n$.

Sometimes people call this the *greatest common factor*. It is also often notated using the horrible notation $(a,b)=n$ which is some serious abuse of notation in my eyes. For our purposes I will write it out fully.

Note the requirement of *largest* there. Even though $4|24$ and $4|16$, the $\gcd(16,24)=8$ as $8$ divides both *and* $8>4$.

It turns out that this is incredibly useful and a lot of powerful results depend on the $\gcd$, however, before we dive into some let us talk about some more terminology.

> **Definition**: If $\gcd(a,b)=1$ then $a,b$ are **relatively prime** or **coprime**

We call these "relatively prime" as the numbers themselves may not be prime, but from the perspective of each other they share no common factors so they might as well be. $\gcd(15,16)=1$ so $15,16$ are relatively prime.

> **Corollary**: $1$ is coprime with every integer

This follows immediately from the definition. This property is basic as fuck though so its more worthwhile to talk about something that's actually something we will use.

> **Theorem**: For all integers $a,b$, $\gcd(a+b,b)=\gcd(a,b)$

Effectively, taking two numbers and adding one to the other will have no impact on the $\gcd$. So $\gcd(25,12)=1$ and so is $\gcd(37,12)$. To prove this, we will take advantage of theorems on the previous page.
{{% callout info %}}
<details>
<summary>Proof</summary>
Let $\gcd(a,b)=d_1$ and $\gcd(a+b,b)=d_2$. To prove we will show that $d_1|d_2$ and $d_2|d_1$ which by anti-symmetry would mean that $d_1=d_2$.
</br></br>First we know that $d_1|a$ and $d_1|b$, which by extension means that $d_1|a+b$. This means $d_1$ is a common divisor of $a+b,b$ and since $d_2$ is the greatest common divisor then $d_1\leq d_2$ and $d_1|d_2$.
</br></br>Now consider $d_2$. We know that $d_2 | b$ and $d_2|a+b$, but since $d_2|b$ we know that $d_2|a$ because otherwise $d_2$ wouldn't divide $a+b$ (per theorem on previous page). Since $d_2|a$ and $d_2|b$ by similar reasoning as before, we can show that $d_2|d_1$. As such, $d_1=d_2$
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

A quick corollary (that I put as a practice problem so won't prove but its easy I promise) is that we can do this for any linear combination!

> **Corollary**: Let $k\geq 0$. For all $a,b$ $\gcd(a+kb, b)=\gcd(a,b)$ 

This will eventually form the basis of the algorithm that we will use to compute the $\gcd$, however before we get into that lets discuss one funny little corollary of this theorem

> **Corollary**: Let $F_n$ be the $n^{\text{th}}$ fibonacci number. $\gcd(F_{n+1}, F_n)=1$
{{% callout info %}}
<details>
<summary>Proof</summary>
We will prove this via induction. To begin, notice that $F_1=1,F_2=1$ are relatively prime as $\gcd(1,1)=1$. We will now perform the induction step, assume that $\gcd(F_n, F_{n-1})=1$. We now can use the previous theorem to see that $$
\gcd(F_{n+1},F_n)=\gcd(F_{n-1}+F_{n},F_n)=\gcd(F_{n-1},F_n)=1.$$
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

There will be a funny little result about this later, but honestly this corrollary isn't that interesting, how about we observe a funny story relating to one of history's biggest goofballs.

---

### Fermat Numbers

Pierre de Fermat, one of the biggest silly gooses of mathematics, was investigating a special type of numbers that we now call "Fermat Numbers"

> **Fermat Numbers**: The $n^{\text{th}}$ Fermat number, notated $F_n$, is given as
$$
F_n = 2^{2^n}+1
$$

The first few Fermat numbers are as follows
$$
\begin{align}
F_0 &= 3 \\\\
F_1 &= 5 \\\\
F_2 &= 17 \\\\
F_3 &= 257 \\\\
F_4 &= 65537.
\end{align}
$$

Interestingly enough, all of these numbers are prime numbers! This led Fermat to conjecture that *every* Fermat number is a prime number, which turned out to be horrifically wrong. This is because literally the very next Fermat number $F_5$ was proved to be not prime by Euler; literally if Fermat had tried one more example he would have disproven his conjecture 😂. Now if that wasn't bad enough, its actually now believe that **no other** Fermat numbers are prime, meaning that Fermat was *infinitely* wrong.

That would have totally been that except for a very interesting property of Fermat numbers that actually make them worthwhile studying

> **Theorem**: For all values $n,m$ such that $m\neq n$, $\gcd(F_m, F_n)=1$.

For this proof we will need polynomial division for one step[^2]
{{% callout info %}}
<details>
<summary>Proof</summary>
Suppose that there exists a $F_n, F_{n+k}$ with $k>0$ such that $d|F_n, d|F_{n+k}$ and $d\neq 1$. Let $x=2^{2^n}$ which means that $F_n=x+1$ and $$
F_{n+k}=2^{2^{n+k}}+1=2^{2^{n}2^k}+1=\left(2^{2^{n}}\right)^{2^k}+1=x^{2^k}+1.$$ 
Conside now what happens when we divide $F_{n+k}-2$ by $F_n$ $$
\frac{F_{n+k}-2}{F_n}=\frac{x^{2^k}-1}{x+1} = x^{2^k-1}-x^{2^k-2}+\ldots - 1.$$
This means that $F_n | F_{n+k}-2$ which means that $d|2\implies d=2$. However, every Fermat number is odd, so $2\nmid F_n$ and $2\nmid F_{n+k}$ which creates a contradiction and proves our claim.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

All Fermat numbers are relatively prime to each other! So while Fermat was initially wrong that this formula would generate new primes straight up, it turns out that in a way he was right, as if you compute and factor the next Fermat number, you'll end up with bigger primes than you had before! Now unfortunately we can't generate large primes this way as we don't have efficient factoring algorithms, however maybe in the future with more advancements we will be able to!

Note that as a corollary we get a secondary proof that there are infinite primes.

There is actually a [webpage](http://www.prothsearch.com/fermat.html) dedicated to tracking the status of Fermat numbers and their prime factorizations. The smallest number that currently doesn't have its factors fully known is $F_{12}=2^{4096}+1$, so if you want to try out some interesting computations try there!

---

### Prime Definition and the LCM

*Complete this at a later date*

---

## An Algorithm for Computing the GCD

Suppose we are given two numbers $a,b$ and are asked to find the $\gcd(a,b)$. Well one super straightforward way that we could do this be to just iterate down, starting at the smaller of the two numbers, and the first number we find that divides them both is the answer. This brute force implementation 
```python
def brute_force_gcd(a,b):
    if a < b:
        a, b = b, a
    
    for i in range(b, 0, -1):
        if (a % i == 0) and (b % i == 0):
            # guaranteed to run when i=1
            return i
```
If we say that $n=b$ then this algorithm runs in $\mathcal{O}(n)$ and $\Omega(1)$[^3]. However, it turns out that we can do better and it comes from our boy Euclid in 300 B.C.

The general idea comes from the following insight. Lets just assume that $b < a$, and now lets divide $a$ by $b$. By the division property we established before, we know
$$
a = q_1b + r_1,
$$
with $0\leq r_1 < b$. However, remember above when we said that $\gcd(a,b)=\gcd(a+bk, b)$? Well we can use this to say
$$
\gcd(a,b) = \gcd(q_1b + r_1, b) = \gcd(b, r_1).
$$
In case it isn't clear how we move from step $2$ to $3$ there, just consider taking the final step in reverse, and adding $b$ to $r_1$ $q_1$ times, and you'll see the whole thing is equal.

Now though $b$ becomes our larger number and we can apply the same process! Writing it all out we find
$$
\begin{align}
a &= q_1b + r_1 \\\\
b &= q_2r_1 + r_2 \\\\
r_1 &= q_3r_2 + r_3 \\\\
&\vdots \\\\
r_{n-2} &= q_nr_{n-1}
\end{align}
$$

Eventually we will have to end up with a step that has $0$ remainder, as we are continually getting smaller and smaller values of $r_j$, but those values cannot go below $0$. This can be done very easily in a recursive way, however I am going to write it using loops

```python
def gcd(a,b):
    # This is using the Euclidean Algorithm
    if a < b:
        # this isn't actually necessary as the algorithm would 
        # correct itself after one step
        # I just think this makes it more clear
        a, b = b, a

    while b != 0:
        r = a % b
        a, b = b, r
    
    return a
```

Surprisingly simple huh! In terms of code complexity this is pretty clearly $\Omega(1)$ as you could just have $b|a$ and be over in one iteration, however what is the Big $O$ of this, and how could you even prove it since this depends on factors of $a,b$?

Well it turns out that this algorithm runs in $\mathcal{O}(\log n)$ where $n=b$ based on this actually absurd theorem.

> **Theorem**: The smallest pair of numbers for which the Euclidean Algorithm takes $n$ steps are $\gcd(F_n, F_{n+1})$ where $F_n$ represents the $n^{\text{th}}$ Fibonacci number.

Wait what the fuck, where do the Fibonacci numbers come from?? The idea is that you will always need to backtrack through the entire Fibonacci sequence to get down to the two numbers having a $\gcd$ of $1$ since every $q_j=1$ and every $r_j=F_j$. As for why these are the first numbers that have this happen, [here](https://en.wikipedia.org/wiki/Euclidean_algorithm#Worst-case) is a proof for now until I get around to writing this proof down on this page in an easy to understand way.

Since the numbers of the Fibonacci sequence grow at a rate of $\mathcal{O}(\phi^n)$, this means that in order to get to $n$ steps you need to travel an exponential distance out, which is where $\mathcal{O}(\log n)$ comes from.

---

## Practice Problems

> **Theorem**: $\gcd(a,b)=a$ iff $a|b$

> **Theorem**: $n$ and $n+1$ are always relatively prime

> **Corollary**: Let $k\geq 0$. For all $a,b$ $\gcd(a+kb, b)=\gcd(a,b)$ 

[^1]: Also the location I got this proof from, although if I ever try to put all this stuff into a real book one day I'll get a real published source. 
[^2]: In case you are wondering how someone thought of this, a lot of trial and error most likely with a bit of creative inspiration
[^3]: Note this is psuedo-polytime as its $\mathcal{O}(n)$ with respect to the *value* of $n$ rather than the number of digits of $n$.