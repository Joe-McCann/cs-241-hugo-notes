---
title: The Forbidden Math 🚫
linktitle: The Forbidden Math 🚫
toc: true
type: book
date: 2023-12-30
draft: false
tags:
    - number theory
    - tiktok
    - digits

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5006
---

## Problem Description

Silly memes have shown the idea of "the forbidden math" in which students could come to the correct answer of a math problem through incorrect assumptions. The original shown is the following.

Find $x$ in $x^2=25$. Cancel out values of $2$, and see that $x=5$. 

This is the correct answer, but obviously not correct as you can't "cancel" the twos, rather taking the square root is correct. Still, a student could make this mistake and still be correct. The question becomes, what other numbers is this false process valid for? In this page we will explore some various cases

---

## Square Roots

### First Digit

Let us first consider the case similar to that as the given example problem, in which instead of taking the square root we "cancel" out the first digit. This actually can be shown in a closed form in that we are looking for an equation so that the square root is just the first digit removed.

> **Problem**: Find all equations $x^2 = n$ such that $n \geq 10$[^1] and $n = 2\cdot 10^m + \sqrt{n}$ with $m$ being the number of digits of $n$ (starting at $0$).

We can see pretty quickly that for $m=1$ our solution of $n=25$ is correct. Can we find any other solutions?

Since we have an equation for the value of $n$ that we are looking for, we can solve this quite easily using the quadratic equation.
$$
\begin{align}
n &= 2\cdot 10^m + \sqrt{n} \\\\
n - \sqrt{n} - 2\cdot 10^m &= 0 \\\\
\sqrt{n} &= \frac{1\pm \sqrt{1+8\cdot 10^m}}{2}
\end{align}
$$
where we get the last step by using the quadratic formula. We will square both sides so we can get the equations into the forms that we are looking for
$$
n = \frac{1+8\cdot 10^m + 1 \pm 2\sqrt{1+8\cdot 10^m}}{4} = 2\cdot 10^m + \frac{1 + \sqrt{1+8\cdot 10^m}}{2}
$$
which should make sense because that's literally the definition of the problem that we stated. We also only consider the positive soluton as thats the only instance that makes sense for the problem statement.

Plugging in $m=1$ we find that $n=25$ which is the solution we know previously. Plugging in $m=2$ we find our answer to be $n=200+\frac{1+\sqrt{801}}{2}\approx 214.65$. 

The fact that this irrational though makes it not as fun, can we do something to see how often this is an integer as a solution? Since $1+8\cdot 10^m$ is always odd, it is enough to ask if $\sqrt{1+8\cdot 10^m}\in\mathbb{Z}$ for any value of $m>1$? Unfortunately as we will show, the answer is *no*.

> **Theorem**: If $m>1$ then $\sqrt{1+8\cdot 10^m}$ is not an integer.
{{% callout info %}}
<details>
<summary>Proof</summary>
This is a proof by contradiction, so first assume that $\sqrt{1+8\cdot 10^m}=q$ where $q$ is an integer. We can rearrange this to see that this means
$$
8\cdot 10^m = q^2-1.$$
$q^2-1$ is a difference of squares, so we can factor it and we can prime factor the $8\cdot 10^m$ as well. This will give us
$$
2^{m+3}5^m = (q-1)(q+1).$$
Since $q$ is odd, we know that this means $\gcd(q-1,q+1)=2$ as the two numbers can only share a common factor between them of $2$. From here we can see that there are only two possible prime factorizations of $q-1,q+1$ that will give us $8\cdot 10^m$. We could have either $2, 2^{m+2}5^m$ or $2^{m+2},2\cdot 5^m$. 
</br>
However, $q-1$ and $q+1$ must have a difference of $2$. The only time that $2, 2^{m+2}5^m$ has a difference of $2$ is when $m=0$, and $2^{m+2},2\cdot 5^m$ when $m=1$, which means that if $m>1$ the prime factorization will not work out which is a contradiction, meaning that $q$ cannot be an integer.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

### First Digit in Other Bases

We can easily expand this to other bases $b>2$ by slightly adjusting the equation. Note that when representing a number in base $b$ I'll state it with a subscript $b$ such as $10_b$.

> **Problem**: Find all integer equations $x^2 = n$ such that $n \geq 10_b$[^1] and $n = 2\cdot b^m + \sqrt{n}$ with $m$ being the number of digits of $n$ (starting at $0$).

Following the same process as the prior example we can get
$$
n = 2\cdot b^m + \frac{1 + \sqrt{1+2^3\cdot b^m}}{2}.
$$

Right off the bat we get this nice lemma that shows us that this is not special to base $10$, rather there are infinite bases that have this problem with solutions.

> **Lemma**: There are infinite bases $b$ for which there exists at least one $m\geq 1$ with an integer solution to the problem
{{% callout info %}}
<details>
<summary>Proof</summary>
We can see that we just need to prove that $x^2 = 1+8b^m$ has infinite integer solutions which is very easy. Just select $m=1$ for sake of argument so we can solve for $b$ in terms of $x$, to see that our equation is now $x^2 = 1+8b$ which we can adjust to $b = \frac{x^2-1}{8}$. Any odd $x\geq 3$ will provide an integer solution for $b$ which proves the claim.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

A more meaningful question to ask now is if it is possible to have multiple solutions occur in the same base for different values of $m$. Clearly, each value of $m$ can only have at most one solution (one equation with one unknown), but can we have a situation where one base has many working values of $m$?

> **Question**: Does there exist a value of $b\in\mathbb{N}$ such that there exists more than one integer solution to $$x^2 = 1+8\cdot b^m$$ with $m\geq 1$.

From [this](https://math.stackexchange.com/questions/4838113/does-there-exist-an-integer-b2-such-that-the-equation-x2-18-cdot-by-ha?noredirect=1#comment10304073_4838113) answer on math stack exchange, the answer is **yes** for $b=6$ both $m=1,2$ are solutions. In the same thread it is shown that for $m\geq 3$ there does not exist any base with a solution.

The bases in which $m=1$ has a solution are all bases of the form $\frac{q^2-1}{8}$ where $q$ is an odd number $>5$, so
$$
6,10,15,21,\ldots
$$
which actually turns out to be the triangular numbers (which can be seen by plugging $2n+1=q$)! The solutions for when $m=2$ is the recurrance equation $b_{j+1}=6b_{j+1}-b_j$ which can be got from Pell's Equation. The solutions are 
$$
6,35,204,\ldots
$$

To close this out we ask the following question

> **Conjecture**: Let $b_{j+2}=6b_{j+1}-b_j$ with $b_0=6$ and $b_1=35$. The only term of this sequence that is a triangular number is $6$.

---

### Any Digits

The previous instance of the problem was easily solvable due to only considering the first digit. However, in the even there are multiple instances of $2$ being removed, can we find more examples? 

By a quick python program
```python
for x in range(11, 10**9):
    y = str(x).replace("2", "")
    if y and x == int(y)**2:
        print(x)
```
we can find that solutions exist with
$$
\begin{align}
x^2 =25 &\implies x = 5 \\\\
x^2 =121&\implies x = 11 \\\\
x^2 =1022121&\implies x = 1011 \\\\
x^2 =1212201&\implies x = 1101 \\\\
x^2 =1221025&\implies x = 1105 \\\\
x^2 =2146225&\implies x = 1465.
\end{align}
$$
This leads in to the next unsolved question

> **Conjecture**: There exist no other integers $n$ such that their square root is the decimal expansion of $n$ with all digits equal to $2$ removed.

[^1]: We restrict ourselves to $\geq 10$ to have the problem make sense within the domain we are working and the problem statement.