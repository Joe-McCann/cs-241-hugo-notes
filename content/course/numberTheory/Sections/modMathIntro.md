---
title: Intro to Modular Arithmetic
linktitle: Intro to Mod
toc: true
type: book
date: 2023-08-27
draft: false
tags:
    - cs241
    - number theory
    - integers
    - modular arithmetic

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 220
---

## The Math of Time

In the Chapter on Set Theory, we discussed the idea of relations[^1] and specifically provided the example of modular equivalence on [this page](/course/settheory/sections/relations). In there we stated that the relation
$$
a\equiv b \mod n
$$
was true if both $a,b$ had the same remainder when divided by $n$. This is true, but now we have more knowledge and the ability to provide more technical details that will allow us to do some pretty cool stuff with modular arithmetic.

Before we get into the weeds though, I find it valuable to provide examples for why modular arithmetic is so valuable. Besides the eventual example that we will get to of encryption schemes, it is also the mathematics of time. Clocks go from $1$ to $12$ continually wrapping back around instead of being like the Unix clock that counts seconds from January 1980. Days of the week go in the same $7$ day cycle. What I'm trying to say is that you're already familiar with the basic ideas of modular arithmetic, but soon will be able to answer questions such as

> **Example**: You take a pill every $3$ days and take your first pill on a Monday. How many pills do you take before the first one occurs on a Tuesday?

Well we can see that it goes Monday, then Thursday, Sunday, Wednesday, Saturday, Tuesday. This means that the sixth pill that you take will be the one on the Tuesday, and in fact after you cycle through $7$ pills (last one is Friday) you will not only start over but will have hit every single day! We could have put numbers on this, if we say that Monday is $0$ and Tuesday is $1$, then our sequence would have gone
$$
0,3,6,2,5,1,4,0,3,\ldots
$$

By the way, if you want to calculate $a\mod n$ using code, the way to do it (in `python3` at least) is `a % n` where `%` is the modulus operator, but if you didn't have access to that you could always do
```python
def mod(a,n):
    return a - int(a/n)
```
which would give you the remainder.

### What Really is mod?

When we discussed mod before, we proved it was an equivalence relation via the definition that two items are related if they have the same remainder, but thats kinda a wavy definition and is hard to really do any work with. As such I will give the true definition, although rest assured I won't make you go through the effort of proving that its an equiv relation again (although you can for practice).

Think about dividing both $a,b$ by $n$. Let
$$
\begin{align}
a &= q_1n + r_1 \\\\
b &= q_2n + r_2.
\end{align}
$$

If we say that the two values have the same remainder when divided by $n$, then $r_1=r_2$ so if we subtract $a,b$ we get
$$
a-b = q_1n-q_2n = n(q_1-q_2)
$$
and notice this is divisible by $n$ because the remainders cancel out! This is how we define the following

> **Definition**: We say that $a\equiv b\mod n$ iff $n|a-n$

We can run through some examples
$$
\begin{align}
6 | 7-1 &\implies 7\equiv 1\mod 6 \\\\
10 | 125-25 &\implies 125\equiv 25 \mod 10 \\\\
2 | (2k+1) - (2j+1) &\implies (2k+1) \equiv (2j+1) \mod 2
\end{align}
$$

Notice that last one is showing that all odd numbers are equivalent $\mod 2$ even when using our definition provided earlier in the chapter.

Also notice that this really doesn't say anything about what numbers we are selecting, so we can use this to say that
$$
5\equiv -1 \mod 6
$$
Since $5-(-1)=6$. Using negative representations is often very very helpful in computations. Also you can funny enough have shit like
$$
2\pi \equiv \pi \mod \pi
$$
if you expand your idea of "divisibility" a bit lmao.

### Algebraic Identities for Mod

Mod is cool and all, but in order to do really any mathematics, we are going to want to do some identities so we can work with it and do some cool stuff later down the line. Lets start with easy shit and then work our way into some harder ones. We will then parlay this into an algorithm to calculate insanely large modulus values.

Lets start with some easy equality stuff

> **Theorem**: If $a\equiv b\mod n$ then $ka\equiv kb\mod n$
{{% callout info %}}
<details>
<summary>Proof</summary>
Since $a\equiv b\mod n$ we know that $n|(a-b)$ so
$$
ka-kb = k(a-b) \implies n | k(a-b)$$
meaning that $ka\equiv kb\mod n$
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

> **Theorem**: If $a\equiv b\mod n$ and $x\equiv y\mod n$ then $ax\equiv by \mod n$
{{% callout info %}}
<details>
<summary>Proof</summary>
We start by seeing that we know $n|a-b$ which means $a-b=nk$ for some value of $k$, we can see $a = nk+b$ then. We now plug this into $ax-by$ to see
$$
ax-by = (nk+b)x-by = nk+bx-by = nk+b(x-y)$$
and since $x\equiv y\mod n$ we know $n | x-y$ so $n | nk+b(x-y) \implies n|ax-by$ and this means that 
$$
ax\equiv by\mod n$$
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

Now it might not be immediately obvious how this can apply to us, but this means that if we wanted to calculate a massive product $\mod n$, we can take the mod of the individual pieces first so we don't need to compute the product! For example
$$
104\cdot 15\mod 3 = (104\mod 3)\cdot (15\mod 3) = 2\cdot 0 = 0
$$
therefore $104\cdot 15\equiv 0\mod 3$ and we didn't need to bother multiplying $104$ and $15$. I'll succinctly write these out at the end.

> **Corollary**: If $a\equiv b\mod n$ then $a^k\equiv b^k\mod n$

This follows directly from the previous theorem so I won't prove it here, but it's a practice problem.

> **Theorem**: If $a\equiv b\mod n$ and $x\equiv y\mod n$ then $a+x\equiv b+y\mod n$

This one is also super easy to prove using the previous techniques, so I'm making it also a practice problem. Now lets discuss something actually interesting. You might be tempted as of right now to think that all types of equality are true in modular arithmetic, but one thing that doesn't hold up is **division**, which we can see here
$$
\begin{align}
4&\equiv 8\mod 4 \\\\
2&\not\equiv 4 \mod 4
\end{align}
$$

Specifically if we see
$$
ka\equiv kb\mod n\not\implies a\equiv b\mod n
$$
because the value of $k$ could actually be adding in additional factors that $a,b$ are missing in order to get them equivilent $\mod n$. An easier example would be if $n=2$, because then we are just checking even vs odd. If $a$ is even and $b$ is odd then they wouldn't be equal, but if you were multiplying by $k=2$ that would make both products even, despite $b$ being odd. What we need to do in order to make division work is *remove all the common factors of $k,n$ from the equation*, which written out is

> **Theorem**: $$
ka\equiv kb\mod n\implies a\equiv b\mod \frac{n}{\gcd(k,n)}$$

This proof is kinda cool, so I am going to write it out instead of putting it in a proof block. 

*Proof:* 

We will first start by saying that $\gcd(n, k)=d$ for brevity. By definition $d|k$ and $d|n$ so
$$
n = n_1d, k=k_1d
$$
where $d\nmid k_1$ and $d\nmid n_1$. This means that $\gcd(n_1, k_1) = 1$. Since $n|ka-kb$ we know that $\frac{ka-kb}{n}$ is an integer. Doing some algebra we have that
$$
\frac{ka-kb}{n}=\frac{k_1da-k_1db}{n_1d} = \frac{k_1a-k_1b}{n_1}=\frac{k_1(a-b)}{n_1}.
$$
We know that $\gcd(k_1,n_1)=1$ which means that since $n_1|k_1(a-b)$ that $n_1|a-b$. This means $a\equiv b\mod n_1$ and since $n_1=\frac{n}{d}$ we can say
$$
a\equiv b\mod \frac{n}{\gcd(k,n)}.
$$

**Q.E.D**

---

## An Algorithm for Computing Mod

Taking all the things that we proved from the previous section, we can now provide us with some slick identities that will allow us to do fast computations of large modulus. For example, what is the last two digits of
$$
156^{1035}-503^{20}?
$$
We will go through some tricks that will help us, and then provide an algorithm so we can compute this ourselves. In this case we use code notation to notate that we are looking to reduce the number $\mod n$

1. `a*b % n = (a % n) * (b % n)` 
2. `(a + b) % n = (a % n) + (b % n)`
3. `a^b % n = (a % n) ^ b`

Theres also a more advanced theorem called the Chinese Remainder Theorem, but we'll discuss that at a later time. Now that we have these tools, we have the ability to provide an algorithm for computation. Let us first focus on reducing $503^{20}$. In order to find the last two digits, this means that we are looking for $\mod 100$ so we can start with
$$
503^{20} \equiv 3^{20} \mod 100
$$
now this can be plugged into the calculator, but we want to do it by hand. What we will do is notice that we can represent $12$ in terms of powers of $2$ so $20 = 16 + 4$. This might not seem useful, but what we can do is the following.

Notice that $3^2 = 3^1\cdot 3^1.$ $3^1\equiv 3\mod 100$ so $3^2\equiv 9\mod 100$. That might seem insanely obvious but I promise this is going somewhere. We can continually repeat this process to get higher powers of $3$.
$$
\begin{align}
3^1 &\equiv 3\mod 100 \\\\
3^2 &\equiv 3\cdot 3 \equiv  9 \mod 100 \\\\
3^4 &\equiv 3^2\cdot 3^2 \equiv 9\cdot 9\equiv 81 \mod 100 \\\\
3^8 &\equiv 3^4\cdot 3^4 \equiv 81\cdot 81 \equiv 61 \mod 100 \\\\
3^{16} &\equiv 3^8\cdot 3^8 \equiv 61\cdot 61\equiv 21 \mod 100
\end{align}
$$

Now in order to get $3^{20}$ we can do that
$$
3^{20}=3^{16+4}=3^{16}\cdot 3^4 \equiv 21\cdot 81 \equiv 1\mod 100.
$$
This shows us that $503^{20}\equiv 1\mod 100$[^2]. Now to do $156^{1035}$, I will go through this much faster. Note that $156\equiv 56\mod 100$
$$
\begin{align}
156^1 &\equiv 56\mod 100 \\\\
156^2 &\equiv 156\cdot 156\equiv 56\cdot 56 \equiv 36\mod 100 \\\\
156^4 &\equiv 156^2\cdot 156^2\equiv 36\cdot 36\equiv 96\mod 100 \\\\
156^8 &\equiv 156^4\cdot 156^4\equiv 96\cdot 96\equiv 16\mod 100 \\\\
156^{16} &\equiv 156^8\cdot 156^8\equiv 16\cdot 16\equiv 56\mod 100 \\\\
156^{32} &\equiv 56\cdot 56\equiv 36 \mod 100 \\\\
156^{64} &\equiv 96\mod 100\\\\
156^{128} &\equiv 16\mod 100\\\\
156^{256} &\equiv 56\mod 100\\\\
156^{512} &\equiv 36\mod 100\\\\
156^{1024} &\equiv 96\mod 100\\\\
\end{align}
$$
Notice that at once point we got $56$ back which creates a cycle. Now we find that
$$
156^{1035} = 156^{1024}\cdot 156^{8}\cdot 156^2\cdot 156^1\equiv 96\cdot 16\cdot 36\cdot 56\equiv 76\mod 100
$$
Therefore we can finally say that
$$
156^{1035}-503^20\equiv 76-1\equiv 75\mod 100
$$
So the last two digits are $75$.

Actually coding this is usually a problem I give to my students, so sorry 😉

---

## Congruence Equations

We started this page with the pills example by asking what value of $x$ we could plug in to satisfy the equation
$$
2x\equiv 1\mod 7.
$$
and found that $x=4$ satisfied (so the fifth day starting at day $0$) the equation. But clearly not all modular equations are in fact solvable, for example
$$
2x \equiv 1\mod 2.
$$
Clearly $2\nmid 2x-1$ for any value of $x$, so that won't work. To close out this page, we will provide a proof for when modular equations are solvable, which will be probably the most brutal proof of this class so far 🔥[^3]. Hold onto your hats kids, we're going out with a bang.

> **Theorem**: The equation
$$
kx\equiv l\mod n
$$
has a solution iff $\gcd(k,n)|l$. If true then there exists $d=\gcd(k,n)$ solutions $x$ such that $0\leq x < n$, and every solution is of the form $x = t+i\frac{n}{d}$ with $i=0,1,2,\ldots$ and some value $t$

Before going into this proof we can see that in the example
$$
2x\equiv 1\mod 7
$$
since $\gcd(2,7)\equiv 1 | 1$, we can say a solution will exist and there will be one specifically (which we saw), but
$$
2x\equiv 1\mod 2
$$
will have solutions as $\gcd(2,2)=2\nmid 1$.

For practice, find all $5$ solutions for the following equation
> **Example** $15x\equiv 10\mod 20$.

Ok, now onto the proof. This proof will be left outside a box so you're force to read it.

*Proof*:

We will start by proving that a solution exists iff $\gcd(k,n)|l$. This is actually quite easy as
$$
kx\equiv l\mod n\implies n | kx-l\implies kx-l = ny\implies kx-ny=l,
$$
and by Bezout's Lemma, we know a solution for $x,y$ exists iff $\gcd(k,n)|l$, proving the first part.

Let $d=\gcd(k,n)$, we will now prove that there are $d$ solutions. Clearly, if $d=1$ then there is one solution and we are just done. Now we will assume that $d > 1$ for the remaining cases. Since we know a solution exists, we know that $d|l$, and since $d|n$ and $d|k$ we can write out that
$$
k=k_1d, n=n_1d, l=l_1d.
$$
Notice that $\gcd(k_1,n_1)=1$ since $d=\gcd(n,k)$, so we know that
$$
k_1x-n_1y = l_1
$$
has a solution and by extension (doing a reverse of the steps in the first part of the proof)
$$
k_1x\equiv l_1 \mod n_1
$$
has only one solution. Lets say that $x=t$ with $0\leq t < n_1$ be our solution we found. Notice that this *also* is a solution to our original equation
$$
kx\equiv l\mod n
$$
as
$$
\frac{kt-l}{n}=\frac{k_1dt-l_1d}{n_1d}=\frac{k_1t-l_1}{n_1}
$$
which we know is an integer since $t$ is a solution to $k_1x\equiv l_1\mod n_1$ so $n|kt-l$.

Since we have found one solution to our equation, now consider values of $x=t+in_1$ where $i=0,1,2,3\ldots$. These values of $x$ must also be solutions to
$$
k_1x\equiv l_1\mod n_1
$$
as $in_1\equiv 0\mod n_1$ for all values of $i$. We can also show that all these values of $x$ are **also** solutions to our original equation as well by doing the same fraction thing we did before (try it, it works). So we now are generating infinite solutions to our initial equation by continually incrementing $i$, the last step is to show that the are only $d$ unique solutions $\mod n$.

Obviously, if you increment $i$ enough times, you will have a repeat solution for $t+in_1$ as there are only $n$ possible different values of modulus, and if $i>20$ by pigeonhole principle we'll have to repeat. So suppose we have two solutions that are the same
$$
t + in_1 \equiv t + jn_1 \mod n \implies in_1 \equiv jn_1\mod n
$$
where $i\neq j$. Then we know that
$$
n | in_1-jn_1 \implies n | n_1(i-j)
$$
now remember that $n = n_1\cdot d$ so this means that
$$
n | n_1(i-j) \implies d | i-j \implies i\equiv j \mod d.
$$
Finally, we can see that if $i,j$ are equivalent $\mod d$ then the solutions will be equivalent $\mod n$ and by the pigeonhole principle there are $d$ possible mod values we can have, which provides us with $d$ possible solutions, all of the form
$$
x = t + in_1
$$

**Q.E.D.**

Holy shit come on be honest is that not a rush completing something like that? By the way, this also give us an algorithm to find all solutions to a modular equation $kx\equiv l \mod n$

1. Use extended Euclidean Algorithm to solve $kx-ny=l$ for some values of $x,y$. Take $x\equiv t\mod n$ such that $0\leq t < n$. Save the $\gcd(k,n)=d$.
2. Find all other solutions $t+id$ for all values $0\leq i < d$

Lets run through the example from previous page to see this idea in action. Solve for all solutions
$$
134x\equiv 12 \mod 120
$$
which to find one solution we solve $134x=12+120y\implies 134x-120y=12$. From the previous page we know that $x=18,y=20$ is a valid solution. The value of $y$ doesn't matter, and plugging $134\cdot 18=2412$ we can see that $2412\equiv 12\mod 120$.

We set $t=18$ and also $\gcd(134,120)=2$ from the same page, which makes $n_1=\frac{120}{2}=60$. Since $d=2$ we know there are two solutions $x=18, 78$.

---

## Practice Problems

> **Example**: Prove that $a\equiv b\mod n$ is an equivalence relation using the true definition of mod

> **Corollary**: If $a\equiv b\mod n$ then $a^k\equiv b^k\mod n$

> **Theorem**: If $a\equiv b\mod n$ and $x\equiv y\mod n$ then $a+x\equiv b+y\mod n$

> **Example**: Find all solutions to $35x\equiv 14\mod 21$

[^1]: You're **still** never done with relations mwahahaha
[^2]: Picking $n=100$ kinda makes my "do it by hand" argument hold less water as you could in theory be doing $99^2$ by hand but if you're clever you can do some FOIL method to simplify it.
[^3]: By 🔥 here I mean you're in hell and totally fucked so good luck.