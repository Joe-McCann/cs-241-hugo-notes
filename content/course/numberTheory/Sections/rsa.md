---
title: RSA Encryption 🔒
linktitle: RSA Encryption
toc: true
type: book
date: 2023-08-29
draft: false
tags:
    - cs241
    - number theory
    - integers
    - modular arithmetic
    - totient
    - RSA
    - algorithms

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 235
---

## What is RSA Encryption 📬

We've gone through a solid amount of number theory so far, and its crazy to think that this actually has a *direct* application in computer science, but it does. Yes, there truly is an algorithm containing Euler's Totient function that you use on the daily.

[RSA Encryption](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) is the most widely used form of public-private key encryption. You might think that **RSA** stands for something cool, but nah its just the names of its inventors. Also, it was invented for the government and now its public and been declassified 🗽 🦅. Funny thing is that supposedly the story is that all three inventors went to a party one night and got super drunk and *R* figured it out when he got home lmao.

When learning about public-private key encryption you probably learned about it in the following way.

Suppose I (Joe) want to get a message from my roommate Anuj, in a way that nobody can break in and see what he's saying. This is quite expected as the two of us are drama llamas and talk a lot of shit about people.

What I will do is send Anuj a box and an unlocked lock, while I will be holding onto the key for that lock. Anuj will get an empty box to put his message into and then lock the box with the lock; if someone was to catch the box before in transit, they would just have an empty box which is useless to them. Anuj then sends me back the locked box with the message to me, and I can unlock the box with the key I saved; anyone who intercepts the box would not be able to unlock it without the key.

But like, how does this *really* work? What is the box and lock and key? We are working with binary encoded messages being sent back and forth so how would you got about programming an algorithm to do this? That is the goal of this page, to explain the algorithm from scratch so you can code it yourself.

---

## The RSA Theorem

Before we can get into the RSA algorithm, we need to explain *why* it works by proving one final theorem. This theorem actually uses material that we built up over the past few pages which is why we needed to wait for so long to get here!

> **RSA Theorem**: Let $p,q$ be prime and $k$ be some integer such that
$$
k \equiv 1 \mod \phi(p\cdot q)
$$
the for every $m\in\mathbb{Z}$
$$
m^k\equiv m \mod pq
$$

This is similar to the Euler-Fermat Theorem in which we are able to raise anything number to a power and then get the original number back. In this way, if you envision $m$ to be our binary message in number form then we can raise it to a power and then get it back. For example $p=11,q=13$, then $\phi(11\cdot 13)=(11-1)(13-1)=120$. So let $k=121$ and $pq=143$ then any value of $m$ has
$$
m^{121}\equiv m\mod 143
$$
Try it out in python it works! Although, it is important to note that if $m \geq 143$ then performing `m**(121) % 143` will provide you with the *least* residue, which is still equivalent but you lose some information. Anyway, lets prove that this works.

*Proof:*

To prove that we have
$$
m^k \equiv m \mod pq
$$
it is actually sufficient to show that both of the following conditions are true
$$
\begin{align}
m^k &\equiv m \mod p \\\\
m^k &\equiv m \mod q.
\end{align}
$$

Since those are the same condition, just with different variables, we can prove it for $p$ and then apply the same argument to $q$ to complete the proof. Since $k\equiv 1\mod \phi(pq)$, we know that
$$
\begin{align}
k&=j\cdot\phi(pq)+1 \\\\
&=j\cdot\phi(pq)+1 \\\\
&=j(p-1)(q-1)+1 \\\\
&=h(p-1)+1
\end{align}
$$
note that $j,h$ are just constants to have divisibility and $h = j(q-1)$. If $p|m$ then $m^k\equiv 0\mod p$ and by definition $m^k\equiv m\mod p$.

If $p\nmid m$ then $\gcd(p,m)=1$ so
$$
m^k = m^{h(p-1)+1} = m\cdot \left(m^{p-1}\right)^h 
$$
and by Fermat's Little Theorem we know that
$$
\left(m^{p-1}\right)^h \equiv m\cdot (1)^h\equiv m\mod p
$$

We can make the same argument for $q$ and as such this proves the claim.

**Q.E.D.**

---

## The RSA Algorithm

We are now ready to describe the RSA Algorithm. Suppose that we have that I want to get a message from my sister, there will be three phases of this algorithm.

1. Pre-calculations
2. Encryption
3. Decryption

For the pre-calculations I will start by choosing two prime numbers $p,q$. We then need to calculate $n=pq$, $\phi(n)=(p-1)(q-1)$. We now need to choose some value of $e$ such that $\gcd(e,\phi(n))=1$. We now find a value of $d$ such that
$$
d\cdot e\equiv 1 \mod\phi(n)
$$
which we know we can do and get by the Extended Euclidean Algorithm.

For our purposes we will use $n,e$ as the public key and $d$ will be our private key. I will send $n,e$ to my sister and she will perform the encryption algorithm. To encrypt she will take the integer that represents her message $m$, and calculate
$$
\sigma \equiv m^e \mod n
$$
where $\sigma$ is the least residue of $m^e$. My sister will then return me $\sigma$.

In order to decrypt the message, I take $\sigma$ and raise it to the power of $d$, which amounts to
$$
\sigma^d\equiv \left(m^e\right)^d\equiv m^{d\cdot e}\equiv m \mod n
$$

Thats it! Its actually super simple, and we are going to work through an example.

### An Example of RSA

Let $p=11$ and $q=13$, we will manually run through an example so you can see how it really works. Let it be known though, in practice these prime numbers that we are dealing with can be several hundred digits long.