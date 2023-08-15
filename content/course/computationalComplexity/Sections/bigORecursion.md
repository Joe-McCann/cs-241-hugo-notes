---
title: How to do Big O with Recursion
linktitle: Recursive Big O
toc: true
type: book
date: 2023-07-03
draft: false
tags:
    - cs241
    - software engineering
    - computational complexity
    - asymptotics
    - big O
    - recursion

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 320
---

## A Victim of my Own Design

Prior, we have been working on very nice and fun programs that have been super straightforward to do counting on, but many algorithms that are taught in classes are self referential and recursive. How can we count operations when our operations refer to themselves?? In case my late night rambling isn't clear, consider this simple code to calculate Fibonacci numbers, which are given by the formula
$$
F_n = F_{n-1}+F_{n-2}
$$
with initial conditions $F_0=0, F_1=1$

```python
def fib(n):
    # return the nth fibbonacci number
    if n <= 1:
        # base cases n=0, n=1
        return n

    return fib(n-1)+fib(n-2)
```

This code will give us the sequence we know and love $0,1,1,2,3,5,8,13,\ldots$. Now lets count some operations to see the runtime of this code. In the event that $n=0$ or $n=1$ we get that $T(n)=2$ by our base case. However, if $n>1$, we find that our operations function has $1$ comparison, $3$ arithmetic, $1$ return, and $2$ function calls. But the two function calls are to the function itself which creates this bizzare situation of
$$
T(n) = T(n-1)+T(n-2)+7.
$$
How can we try to classify this into some $\mathcal{O}$? Can we come up with a clean expression for it?

The answer is **yes**, but we need to learn a new technique.

---

## Recurrance Relations

Whenever we have a function that relates back to itself in some previous case, we call this a **recurrance relation**. For example, the Fibonacci sequence itself is a recurrance relation, as we say that $F_n=F_{n-1}+F_{n-2}$, which shows us that in order to solve for a value of $F_n$ we need to know what comes before it. These equations also come with initial conditions that actually change how the sequence unfolds over time.

Now lets actually show how to solve these sorts of equations.

### First Order Linear Equations

Before we get into the actual example that we want, lets consider some easier cases of recurrance relations that we can easily solve by patter recognition. First, lets consider the example

> **Example**: Let $f_n = 2f_{n-1}$ with $f_0=1$. Find an equation in terms of $n$ for $f_n$

Well let's plug in some values and see what happens
$$
\begin{align}
f_0 &= 1 \\\\
f_1 &= 2f_0 = 2 \\\\
f_2 &= 2f_1 = 4 \\\\
f_3 &= 2f_2 = 8 \\\\
&\vdots
\end{align}
$$
Hopefully you can see the pattern here in that every iteration we are adding a factor of $2$ which gives us that $f_n=2^n$. But what if we weren't multiplying by $2$, and what if we had a different initial condition?

This type of recurrance relation is called a *first order* relation because it only goes back one step into $n-1$, and since we are continually multiplying by the same number over and over again, we can assume that our solution probably should look something like $f_n=Cr^n$ where $C$ is some constant that depends on the initial condition. Why we can make this assumption should be fairly obvious in this first order case (multiply by the same number $n$ times gives $r^n$), but I will justify it later on with more advanced, abliet uneccessary for CS, math.

Lets consider a more general example

> **Example**: Let $f_n = kf_{n-1}$ with initial condition $f_0$. Find an equation in terms of $n$ for $f_n$

Well lets assume that $f_n = Cr^n$ where $C, r$ are some constants that solve the problem. Lets plug these in to the equations, and notice that we can say that
$$
f_n = Cr^n
$$
and that
$$
f_{n-1} = Cr^{n-1}.
$$
Plugging these into our recurrance relation we can solve for $r$.
$$
\begin{align}
f_n &= kf_{n-1} \\\\
\implies Cr^n &= kCr^{n-1} \\\\
\implies \frac{Cr^n}{Cr^{n-1}} &= \frac{kCr^{n-1}}{Cr^{n-1}} \\\\
\implies r &= k.
\end{align}
$$
And just like that we know that $f_n = Ck^n$ by solving for $r$! Now in order to find $C$ we just plug in our initial conditions
$$
f_0 = Ck^0 = C,
$$
which gives us our final solution of
$$
f_n = f_0\cdot k^n.
$$

Ok thats cool, but also seems pretty simple, lets move on to something more "challenging"

---

### Second Order Linear Equations

The vast majority of the time, we will not have only one term in our recurrance relation, so we need to have a method to solve higher order equations that go back more steps than just one at every point. Lets start by working with the Fibonacci sequence

> **Example**: Define the Fibonacci sequence as $F_n = F_{n-1} + F_{n-2}$ with initial conditions $F_0=0, F_1=1$. Find an equation for $F_n$ in terms of $n$.

To begin, we are going to make the same assumption we did in the first order case in which we assume that $F_n = Cr^n$. This might seem like a weird assumption but I promise it will work. When we plug this in to our equation with $F_{n-1}=Cr^{n-1}$ and $F_{n-2}=Cr^{n-2}$ then we get
$$
\begin{align}
Cr^n &= Cr^{n-1} + Cr^{n-2} \\\\
\implies r^2 &= r + 1 \\\\
\implies r^2 - r - 1 &= 0
\end{align}
$$
Woah now it turns our that we have a second degree polynomial for $r$ instead of just a number, which means that we actually will have *two solutions* to find! In fact, when we apply this process to an $n$ order equation, its exactly the same except we will end up with an $n$ degree polnomial!

To solve for $r$, plug it in to the quadratic formula (or wolfram alpha) to find that
$$
r_1 = \frac{1+\sqrt{5}}{2}, r_2 = \frac{1-\sqrt{5}}{2}.
$$
In fact, $r_1$ is normally notated as $\phi$ and has a special name, the **golden ratio**! Also known as the most overrated thing in all of mathematics. Anyway, lets solve for constants, we know that
$$
F_0 = 0 = C_1 + C_2 \implies C_2 = -C_1
$$
and that
$$
F_1 = 1 = C_1r_1 + C_2r_2 = C_1r_1-C_1r_2= C_1(r_1-r_2).
$$
Solving for $C_1$ we get that
$$
C_1 = \frac{1}{r_1-r_2} = \frac{1}{\frac{1+\sqrt{5}}{2}-\frac{1-\sqrt{5}}{2}} = \frac{1}{\sqrt{5}}.
$$
As such we also know that $C_2 = -\frac{1}{\sqrt{5}}$. Adding these into our final equation we get that we can express the Fibonacci numbers as
$$
F_n = \frac{1}{\sqrt{5}}\left(\frac{1+\sqrt{5}}{2}\right)^n-\frac{1}{\sqrt{5}}\left(\frac{1-\sqrt{5}}{2}\right)^n.
$$

This is so cool because (in theory) it means that if you want to express any particular Fibonacci number, you can just plug in that number into this equation and you'll get the corresponding $F_n$. Also crazy because every one of these terms are irrational numbers and yet when you add them together the irrational parts cancel out and you always end up with an integer! 

**Note that when you are dealing with an recurrance relation of order $n$, you are guaranteed $n$ linearly independent solutions**[^1]. Linearly independent means that the solutions are not the same with respect to a constant, so like $3\cdot 2^n$ and $2^n$ are not independent. This will be important later.

---

### Why Can We Assume $r^n$?

This is going to be a semi-technical discussion into why even in higher order equations we can assume $r^n$ and then get a corresponding solution. Also for the purposes of discussion, we will be using our previous example of the Fibonnachi numbers, but this also generalizes to higher order linear equations.

Take note of how we specifically said *linear* equations. The *linear* here actually matters a lot, as linear means that for any two recurrance terms such as $F_{n-1}, F_{n-2}$, we are **only** adding them or multiplying them by a constant. So something like
$$
f_n = 5f_{n-1}-2f_{n-2}+\frac{1}{2}f_{n-6}
$$
would be considered a valid linear equation that we can solve using the methods described in this page, however something like
$$
f_n = f_{n-1}f_{n-2}
$$
would be *non-linear* and thus full of pain when trying to describe its behavior over time.

Not only this, but notice that we don't just have one equation, we actually have two! In order to see this, let me show a function in Python that will calculate Fibonacci numbers

```python
def fib(n):
    if n == 0 or n == 1:
        return n
    
    old = 0
    new = 1
    for i in range(n-1):
        temp = new
        new = new + old
        old = temp
    
    return new
```

Yes, we have the normal, but we also have an equation to set *new* to the sum of the previous *new* and *old*, and the *old* to the previous *new*. We can refer to these in terms of $f_n, f_{n-1}, f_{n-2}$. So writing this out we could say
$$
\begin{align}
f_n &= f_{n-1}+f_{n-2} \\\\
f_{n-1} &= f_{n-1}.
\end{align}
$$
Heres the crazy thing, when we have a system of coupled linear equations, we can use the tools of **linear algebra**. Converting these equations into a matrix we get the system
$$
\begin{bmatrix} f_n \\\\ f_{n-1}\end{bmatrix} = \begin{bmatrix} 1&1\\\\ 1&0 \end{bmatrix}\begin{bmatrix} f_{n-1} \\\\ f_{n-2}\end{bmatrix}.
$$

If we notate this in matrix equation form, then we get
$$
\vec{f}_n = A\vec{f}_{n-1}
$$
where $\vec{f}_n$ is the vector containing $f_n, f_{n-1}$ and $A$ is a matrix containing information on the recurrance relation. Well at this point this looks *literally* identical to the first order equation, so we know that our final solution would end up being
$$
\vec{f}_n = A^n\vec{f}_0.
$$

Ok, like but *why* does this mean that we can solve using $r^n$? Well remember from linear algebra when we could diagonalize a matrix into a $PDP^{-1}$, where $D$ is a diagonal matrix of the eigenvalues of $A$? Well if not its ok but if you do that you get that
$$
\vec{f}_n = P\begin{bmatrix}r_1^n & 0 \\\\ 0 & r_2^n\end{bmatrix}P^{-1}\vec{f}_0,
$$
and if you expand this out for the particular values of $P,P^{-1}$ you'll get an equation for $f_n$ in terms of powers of $r_1,r_2$!!

Now this assumes that all the roots are unique and the matrix is diagonalizable though...

---

### Repeated Roots in Recurrance Relations

Consider the following piece of code for computing a factorial
```python
def factorial(n):
    if n == 0:
        return 1
    return factorial(n-1)
```

Cutting to the chase, this function has operations $T(n) = T(n-1) + 4$ with $T(0)=2$. How do we solve this equation with that constant term inside of there? What we are going to do is employ a super clever trick, and subtract the equation by the other equation $T(n-1)=T(n-2)+4$ to cancel out the terms. We get
$$
\begin{align}
T(n)-T(n-1) &= \left(T(n-1)+4\right)-\left(T(n-2)+4\right)\\\\
\implies T(n)-T(n-1) &= T(n-1) - T(n-2) \\\\
\implies T(n) &= 2T(n-1) - T(n-2)
\end{align}
$$
and this is a second order recurrance relation that we can solve. Note that we now need a second initial condition which we get as $T(1)=6$ by plugging in for $n=1$. Since its a second order equation we expect $2$ linearly independent solutions, and solving this recurrance relation we get to assume $T(n)=Cr^n$ giving us
$$
\begin{align}
Cr^n &= 2Cr^{n-1} - Cr^{n-2} \\\\
\implies r^2 &= 2r - 1 \\\\
\implies (r-1)^2 &= 0
\end{align}
$$
which gives us that $r=1$. We can't just have this though cause we need two independent solutions, also $C\cdot 1^n$ clearly is not the answer. As such, in order to get a second independent solution, we multiply our repeated root by $n$[^2] to give us the roots
$$
T(n) = C_1\cdot 1^n + C_2 n\cdot 1^n = C_1 + C_2 n.
$$
In general, if a root is repeated $k$ times, you would multiply it by $1,n,n^2,n^3,\ldots, n^{k-1}$. Anyway, the fact that this is our answer actually is pretty obvious when you plug values in to our initial recurrance relation, as we're just adding $4$ every single time. Solving for our constants we get
$$
T(n) = 4n+2,
$$
which is $T(n)\in\Theta(n)$ as should be expected for this algorithm.

{{% callout info %}}
Note that you can even have complex roots depending on the values of your recurrance relation! Nothing is off the table 😈
{{% /callout %}}

---

### Solving for $T(n)$ of `fib(n)`

Now we have enough to solve for the recurrance relation shown at the start of this page for Fibonacci numbers.
$$
T(n) = T(n-1)+T(n-2)+7,
$$
which we subtract both sides by $T(n-1)=T(n-2)+T(n-3)+7$ to get rid of the constant to get
$$
T(n)-T(n-1) = \left(T(n-1)+T(n-2)+7\right) - \left(T(n-2)+T(n-3)+7\right)
$$
which simplified down gives us
$$
T(n) = 2T(n-1) - T(n-3)
$$
with additional initial condition $T(2)=0+1+7=8$. Assuming that $T(n)=Cr^n$ and simplifying we get that
$$
r^3 = 2r^2 - 1 \implies r^3-2r^2+1=0.
$$
For this polynomial we get our solution is $r_1=1$, $r_2=\frac{1+\sqrt{5}}{2}$, and $r_3=\frac{1-\sqrt{5}}{2}$. This means that our solution is
$$
T(n)=C_1 + C_2\left(\frac{1+\sqrt{5}}{2}\right)^n + C_3\left(\frac{1-\sqrt{5}}{2}\right)^n.
$$
We could very much solve for the coefficients at this point, but it would be soooooo annoying, so we'll skip it and analyze the equation. Since $C_1$ is a constant and $-1 < \frac{1-\sqrt{5}}{2} < 0$, we can see that as $n$ gets very very large, the only term that matters is $C_2\left(\frac{1+\sqrt{5}}{2}\right)^n=C_1\phi^n$ since $\phi$ is the golden ratio. This means that
$$
T(n)\in\Theta\left(\phi^n\right).
$$

This time complexity is exponential which is terrible! But on this same page we also use a loop to calculate Fibonacci numbers, and if you do the calculations, you would find it is $\Theta(n)$. This is interesting because calculating this out is the key idea behind dynamic programming, which is applied strong induction 😄. 

---

## Divide and Conquer Algorithms

Wait a second, there's another type of recursive. What happenes when we don't go back some fixed amount, but instead divide our input by some amount? This is called a *Divide and Conquer* algorithm. The best example of this is to observe what happens when we do a merge sort. Lets look at some code for a merge sort so we can show whats going on[^3].

```python 
def merge_sort(arr):
    n = len(arr)
    if n == 0 or n == 1:
        return arr
    
    left = merge_sort(arr[0:n//2])
    right = merge_sort(arr[n//2:n])

    i = 0
    j = 0
    n_left = len(left)
    n_right = len(right)
    while (i < n_left) and (j < n_right):
        if left[i] < right[j]:
            arr[i+j] = left[i]
            i += 1
        else:
            arr[i+j] = right[j]
            j += 1
    while (i < n_left):
        arr[i+j] = left[i]
        i += 1
    while (j < n_right):
        arr[i+j] = right[j]
        j += 1
    
    return arr
```

Honestly I'm proud of myself for being able to write a merge sort from scratch when this exhausted, currently I'm in a hotel with no internet in the middle of nowhere Pennsylvania for a family reunion. Normally I wouldn't write something like this in the notes, but I don't think any of my students are going to actually read this page and bring it up in class lmfao.

Anyway, outside of the recursive calls, the function performs $\Theta(n)$ work, you can count yourself for best and worst possible cases to very for yourself if you'd like. Now, the function recursively calls itself twice, but both times require only to sort a list of half the size, to give us the recurrance relation
$$
T(n) = 2T\left(\frac{n}{2}\right) + \Theta(n).
$$
Notice that if we were being very careful we would be careful about integer division `n//2`, but this is good enough for our purposes.

What sort of techniques can we use for this? Can we assume $r^n$? Unfortunately that will not work, as there isn't a super easy way to come up with a formula for these sorts of equations, even if we had a explicity formula there for the merge operations. Rather, we will need to use a theorem that will give us a good way of stating the growth rate.

### The Master Theorem

What I am about to show you is the most insane, balls to the walls theorem that you will ever see. The cases behind it are freaking crazy, and the proof is even more nuts. This theorem, called the *Master Theorem*, tells us how function asymptotics behave depending on their structure.

This is kinda more of a theorem to look at to explain, but I'll provide some examples

> **The Master Theorem**: Let $$T(n) = aT\left(\frac{n}{b}\right) + f(n)$$ where $a$ is the number of divisions per call, $b$ is how much you are dividing the input, and $f(n)$ is the work per call. Let $\varepsilon = \log_b(a)$, then
1. If $f(n)\in\mathcal{O}(n^k)$, with $k < \varepsilon$ then $T(n)\in\Theta(n^\varepsilon)$
2. If $f(n)\in\Theta(n^k)$, with $k = \varepsilon$ then $T(n)\in\Theta(n^\varepsilon\log(n))$
3. If $f(n)\in\Omega(n^k)$, with $k > \varepsilon$ **and** $$0\leq\lim_{n\rightarrow\infty}\frac{af\left(\frac{n}{b}\right)}{f(n)} < 1,$$ then $T(n)\in\Theta(f(n))$

Note that this theorem applies for both our practical and our true version of big $O$, but you just need to make sure that you are consistent.

The whole idea of this theorem hinges on the value $\log_b(a)$ which represents the ratio of how many times we are splitting compared to how small we are dividing up the problem. Based on this and the amount of work we do every iteration, we can come up with our bounds. Note that the proof is based off the idea that you have an exact expression for the sum, and then find the asymptotics. "Exact" is a little loosey goosey here, but there are more exact methods for coming up with the Big $O$ of these, this is just the simplest. Lets run through examples of the cases.

> **Example**: What are the asymptotics of $T(n)=9T\left(\frac{n}{3}\right)+n$
{{% callout info %}}
<details>
<summary>Proof</summary>
Since $a=9$ and $b=3$ then $\log_3(9)=2$. $f(n)\in\Theta(n)$ which means that $k=1$. Since $k < \varepsilon$ then we are in Case $1$, which means that $T(n)\in\Theta(n^2)$
</br>
QED
</details>
{{% /callout %}}

Case $1$ represents the situation in which there is just so much splitting that it completely overpowers all the work that actually gets done. If you represent a recursion tree, then its too many leaves.

> **Example**: What are the asymptotics of merge sort with $T(n)=2T\left(\frac{n}{2}\right)+f(n)$ with $f(n)\in\Theta(n)$?
{{% callout info %}}
<details>
<summary>Proof</summary>
Since $a=2$ and $b=2$ then $\log_2(2)=1$. $f(n)\in\Theta(n)$ which means that $k=1$. Since $k = \varepsilon$ then we are in Case $2$, which means that $T(n)\in\Theta(n\log(n))$
</br>
QED
</details>
{{% /callout %}}

Case $2$ represents the situation in which the splitting and the work done in the calls perfectly balances out to add just a little bit of extra work. As you can see, this shows us how merge sort is $\mathcal{O}(n\log n)$!!

> **Example**: What are the asymptotics of $T(n)=2T\left(\frac{n}{2}\right)+n^4$
{{% callout info %}}
<details>
<summary>Proof</summary>
Since $a=2$ and $b=2$ then $\log_2(2)=1$. $f(n)\in\Theta(n^4)$ which means that $k=4$. Since $k > \varepsilon$ then we are in Case $3$, which means that $T(n)\in\Theta(n^4)$ since the additional constraint of
$$
\lim_{n\rightarrow\infty} \frac{2\left(\frac{n}{2}\right)^4}{n^4} = \frac{1}{8} < 1
$$
</br>
QED
</details>
{{% /callout %}}

Case $3$ represents the case in which there is so much work every iteration that it dominates the splitting. Also, we need that extra condition to make sure that we don't completely blow up our asymptotics from above.

---

### The Master's Master

*In this section I will give the Akra-Brazzi method which is a more general example of the Master Theorem.*

### Strange Multiplication

*Here I will give a weird example of multiplication that uses master theorem*

[^1]: This is the first piece of witchcraft on this page
[^2]: This is the second piece of witchcraft on this page
[^3]: Apologies if this section is not coherent, I'm pretty tired and theres some true crime murder mystery thing that keeps distracting me