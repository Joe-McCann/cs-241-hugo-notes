---
title: Larger Size of Infinity
linktitle: Larger Size of Infinity
toc: true
type: book
date: 2023-06-17
draft: false
tags:
    - cs241
    - sets
    - infinity
    - functions

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 255
---

## The Rationals and the Reals

On the previous page we showed (kinda) how the rationals $\mathbb{Q}$ was a countable set the same size as the naturals $\mathbb{N}$. Right now we will take some time to how $\mathbb{Q}$ is quite intertwined with the set of real numbers $\mathbb{R}$. As we know, $\mathbb{Q}\subset\mathbb{R}$, but there are some numbers in $\mathbb{R}$ that are *irrational* and not in $\mathbb{Q}$, for example $\pi$. From this we can see pretty clearly that $|\mathbb{Q}|\leq |\mathbb{R}|$. The proof of the true relation is pretty involved though, so let's consider some things that might assist our intuition.

### Density

Let's begin by looking at a theorem that should be fairly obvious

> **Theorem**: Let $a,b\in\mathbb{Q}$ and $a\neq b$. Then there exists a $x\in\mathbb{R}$ such that $a<x<b$

This theorem is saying that between any two rational numbers, we can find a real number. Intuitively this should be pretty obvious as any rational number itself is a real number, so we can just take the average of these two rationals

{{% callout info %}}
<details>
<summary>Proof</summary>
Consider $x=\frac{a+b}{2}$. Since adding two fractions and dividing by $2$ will just give another fraction, then we know that $x\in\mathbb{Q}$ which by extension means that $x\in\mathbb{R}$
</br>
QED
</details>
{{% /callout %}}

What is more interesting is the following theorem that says if I pick any two real numbers, I can find a rational in between them!

> **Theorem**: Let $x,y\in\mathbb{R}$ and $x\neq y$. Then there exists a $a\in\mathbb{R}$ such that $x<a<y$

So far we've seen some weird stuff come from infinity so this shouldn't feel too strange, but lets actually prove it

{{% callout info %}}
<details>
<summary>Proof</summary>
To begin, if $x<0$ and $y>0$ then we can choose $a=0$. We will prove this for $x,y>0$ but the proof can be modified for the case in which they are both less. </br>
Let $\varepsilon=y-x$, and choose $n\in\mathbb{N}$ such that $\frac{1}{n}< \varepsilon$ (the fact we can do this is called the Archimedean property). Now let $m\in\mathbb{N}$ be the smallest value of $m$ such that $\frac{m}{n} < x$. Since $\frac{1}{n}< \varepsilon=y-x$, then we know that $\frac{m}{n}+\frac{1}{n} < y$. But we know that $m$ was the smallest value such that $\frac{m}{n}< x$ so $\frac{m+1}{n}>x$. Let $a=\frac{m+1}{n}$ which implies $x< a< y$.
</br>
QED
</details>
{{% /callout %}}

Ok cool, so we can see that picking any two real numbers will have a rational between, and any two rationals will have a real in between. You might now think that this means the number line looks like *rational-real-rational-real-...*, which would totally be a valid way to think, however, the truth of the matter is that you are about to encounter one of the craziest mathematical results in history

---

## The Reals and Uncountability

One of the most famous theorems in all of mathematics has to do with the cardinality of the reals being *uncountable*

>**Theorem (Uncountability of Reals):** $|\mathbb{Q}|<|\mathbb{R}|$

Wait uh what the fuck? How is this possible we literally showed that between two reals there is a rational?? Yep, this is one of those mind boggling results that is tough to wrap your brain around. One way to envision why this might be true is to think of the sequence of rational numbers
$$
3, 3.1, 3.14, 3.141, 3.1415,\ldots
$$
where each term in the sequence adds a new digit of $\pi$. Clearly this is an infinite sequence of rationals (and by extension reals) that converges to a number that is not rational. If we change this sequence at **any single digit** though we'll converge to a brand new irrational number, and then we can change that sequence somewhere, and that sequence. There are an infinite amount of infinite sequences in which we have infinite choices to change, and the fact that there are infinite reals and irrational numbers which have infinite non-repeating digits is the key to our proof.

Before we prove the main theorem though, let us first show a quick mini-theorem we will use to help us

> **Lemma**: $|\mathbb{R}|=|(0,1)|$

{{% callout info %}}
<details>
<summary>Proof</summary>
Consider the function $f:\mathbb{R}\rightarrow (0,1)$ as $f(x) = \frac{\arctan(x)}{\pi}+\frac{1}{2}$. This function is bijective as it has inverse function on this domain $f^{-1}(x)=\tan\left(\pi x-\frac{\pi}{2}\right)$
</br>
QED
</details>
{{% /callout %}}

With this info, instead of solving out the uncountability theorem as its written, we can instead show that $|\mathbb{N}|<|(0,1)|$ since $|\mathbb{N}|=|\mathbb{Q}|$ and $|\mathbb{R}|=|(0,1)|$.

### Cantor's Diagonal Proof

This was not the first way Georg Cantor proved this result, however his first proof was a bit more confusing so mathematicians at the time bullied him so hard that he came up with one of the most iconic proofs in all of math history thinking it would make them believe him[^1].

In order to show that $|\mathbb{N}|<|(0,1)|$, we just need to show that if we have any function $f:\mathbb{N}\rightarrow (0,1)$, that $f$ will **never** be surjective. Remember that surjective means that the function completely covers the codomain with the domain, so if $f$ can never be surjective, that means that no function can ever hit every number in $(0,1)$. This would prove the claim.

For the sake of argument (and eventually contradiction), assume that we have have some function $f$ that is surjective. This means that if I plug in every natural number, I will eventually get out **every** number in $(0,1)$. Just to visualize, lets plug in natural numbers and write out what our function could in theory output.
$$
\begin{align}
f(0)&=.\color{red}{4}\color{white}58429\ldots \\\\
f(1)&=.5\color{red}3\color{white}8828\ldots \\\\
f(2)&=.19\color{red}5\color{white}820\ldots \\\\
f(3)&=.775\color{red}9\color{white}37\ldots \\\\
f(4)&=.9999\color{red}9\color{white}1\ldots \\\\
f(5)&=.96592\color{red}9\color{white}\ldots \\\\
&\vdots
\end{align}
$$

Remember that every input is a natural number, and every output is a real number in $(0,1)$. Also, the actual digits we used here don't matter, we could have chosen any this is just for visualizing. However, since $f$ is surjective, **every value in $(0,1)$ must be somewhere in this output list**. This is a **very important** piece of the proof, so please understand before moving on.

Now notice that I highlighted specific decimal digits of each number in red. Specifically, for $f(n)$ I highlighted the $n^{\text{th}}$ digit (if we start at $0$), and do this infinitely for every $f(n)$. We are going to create a new number $x$ which we will define as follows: the $n^{\text{th}}$ digit of $x$ is the $n^{\text{th}}$ digit of $f(n)$. In easier terms, we are creating $x$ by taking all the digits that we highlighted, so in our example
$$
x = .435999\ldots
$$

See that by how we defined $x$, its $n^{\text{th}}$ digit is always the same as $f(n)$ $n^{\text{th}}$ digit. Lets create a brand new number $y$ by taking $x$ and modifying it a bit: for every digit of $x$, add $1$, and if the digit is $9$, make it $0$. So since for us we know $x=.435999\ldots$, we can see that
$$
y = .546000\ldots
$$

Think about the value $y$ for a second. By definition, $y$ is different than $x$ in every single decimal place. But remember that we said that $x$ has the same $n^{\text{th}}$ digit as $f(n)$. This means that $y$ **must** have a different $n^{\text{th}}$ digit than $f(n)$ for **every** value of $n$. But wait, this means now $y$ actually cannot be an output of $f(n)$, because $y$ differs in at least one decimal point for every value of $n$! This means that $f$ cannot be surjective which contadicts our point that it was, therefore no surjective functions can exist!

As such $|\mathbb{N}|<|(0,1)|$ and by extension $|\mathbb{Q}|<|\mathbb{R}|$. **Q.E.D.**

For those of you who take Analysis and measure theory in the future, it turns out that there are way, *way*, more real numbers than rational numbers. In fact, if you were to take the "percentage"[^2] of rationals vs reals, you would find that the rationals make up $0\%$ of the real numbers! This might seems fairly abstract, but there are real consequences to this!

### General Results

A natural question to ask from here is if $\mathbb{R}$ is the biggest infinite set that we can come up, which is another question that Cantor answered for us with what is called **Cantor's Theorem**

> **Cantor's Theorem**: Let $A$ be a set. Then $|A|<|\mathcal{P}(A)|$

This actually shows us what spawned a whole other side field of ordinal numbers, as this means that there is *no largest set*! We can keep going to
$$
|\mathbb{R}| < |\mathcal{P}(\mathbb{R})| < |\mathcal{P}(\mathcal{P}(\mathbb{R}))| < \ldots
$$
In fact it can be shown that $|\mathbb{R}|=|\mathcal{P}(\mathbb{N})|$, but thats outside the scope of what we will show here.

Well ok, so we can't have a largest infinite set, but is there a smallest infinite set? It turns out there is (kinda)!

> **Smallest Infinity Theorem**: Let $A$ be an infinite set with $|A|=|\mathbb{N}|$. If $B$ is an infinite set and $B\subseteq A$ then $|B|=|\mathbb{N}|$

This theorem shows us that $|\mathbb{N}|$ is the smallest size of infinity, because if we have any infinite subsets of it then they are the same size! This result is actually quite powerful, as from it we can sets like the prime numbers must also be countable as they are an infinite subset of the naturals.

---

## A Real World Consequence of Uncountability for Computer Scientists

Woah, not just a real world consequence, but a real world consequence for computer scientists specifically? Shit its like Christmas 🎅. The following material will be taught to you in CS $341$ (if you're an NJIT student), but its always fun to see how things can apply even if I abstract away some technical details.

When we program, we are given some problem, and from there we have to write an algorithm to solve said problem. Lets look at a specific class of problems and define a way to represent them.

> **Definition**: Let $B$ be the set of all finite binary sequences $\\{0,1,10,11,01,00,\ldots \\}$. Note that we allow for leading $0$s here.

> **Definition**: A **decision problem** is set of finite binary strings $D=\\{t_1,t_2,\ldots\\}$ with $t_k\in B$. If some algorithm $A$ *decides* or *solves* $D$, then $A$ should output **TRUE** for any input string $t\in D$ and **FALSE** if $t\not\in D$

Pretty much we are saying that a problem is defined as the set of all the inputs that you return true for. A decision problem $D$ for example could be *"the set of all inputs that start with $1$"* in which
$$
D=\\{1,10,11,101,110,\ldots \\}.
$$

Clearly, every decision problem $D\subset B$, as $B$ itself is just the decision problem that always returns true. In fact with this we can actually define the set of all possible decision problems

> **Theorem**: Let $\mathbb{D}$ be the set of all decision problems, then $\mathbb{D}=\mathcal{P}(B)

This is pretty straightforward from the definitions of $B$ and $D$. It turns out that $|B|=|\mathbb{N}|$ (which is a practice problem to prove), so by **Cantor's Theorem** we know that $|\mathbb{N}|<|\mathbb{D}|$.

Now lets define what an algorithm is. This definition is actually missing a lot of technical details, but it will do us well enough to get us where we need to go.

> **Definition**: An algorithm $A$ is a finite sequence of characters that describes the steps the algorithm takes.

Again, this is super abridged, but one thing we can do with this is show

> **Theorem**: Let $\mathbb{A}$ be the set of all algorithms. $|\mathbb{A}|=|\mathbb{N}|$

If we use the **Smallest Infinity Theorem** this proof is super easy. Take the description of any algorithm and convert it into its ascii representation, which will give you a number in base $2$. Two different sequences of characters will give you different numbers, so this is a subset of $|\mathbb{N}|$, which immediately means that $|\mathbb{A}|=|\mathbb{N}|$ since there are infinite algorithms.

Think about what this means though, we can now see that $|\mathbb{A}|<|\mathbb{D}|$ which tells us there are more computer problems than exist possible algorithms, so there must be problems that computers cannot solve! Even worse, $|\mathbb{D}|$ is uncountable which means that nearly every problem is impossible to solve, and we are just working in a very, very small subset of problems.

You will learn more about this in CS$341$, but the most famous of these problems is called the **Halting Problem** and was discovered by Alan Turing in the mid $1900$s.

## Practice Problems

> **Theorem**: $|B|=|\mathbb{N}|$ where $B$ is the set of all finite binary strings.

[^1]: It didn't
[^2]: Theres more technical theory here I'm just grazing over it for brevity