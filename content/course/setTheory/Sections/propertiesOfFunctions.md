---
title: Properties of Functions
linktitle: Properties of Functions
toc: true
type: book
date: 2023-01-24
draft: false
tags:
    - cs241
    - functions
    - function properties

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 235
---

## Some of Math's Coolest Terms

We have the general idea of functions, but as with anything in math, we can define some useful properties to have.

### Injectivity

We have as a propery of functions that every input must have exactly one output, but what if that output is always unique to the given input? What I mean is that it is not possible for two different inputs to give the same output? This would be called injective (or one-to-one) and is our first property.

> **Definition**: A function $f(x)$ is **injective** if for all $x,y$ $f(x)=f(y)$ iff $x=y$.

What this is saying is that the *only way* we can have the function give the same output, is if the function has the same input! Lets say that we have sets $A=\\{1,2,3\\}$ and $B=\\{1,2,3,4\\}$ and define function $f(x)$ to be

$$
    \begin{aligned}
        f(1)&=3 \\\\
        f(2)&=2 \\\\
        f(3)&=4.
    \end{aligned}
$$

We can see that $f$ is injective because the same output is never repeated. Here is an example of a non injective function.

> **Example**: Let $\sin:\mathbb{R}\rightarrow\mathbb{R}$. Prove $\sin(x)$ is not injective
{{% callout info %}}
<details>
<summary>Proof</summary>
In order to show that this is not injective, we just need to show that we can find two different inputs of $\sin$ that have the same output. Since we know this is a $2\pi$ periodic function its easy to show by seeing that $\sin(0)=\sin(2\pi)=0$. Therefore $\sin$ is not injective.
</details>
{{% /callout %}}

Notice that since injective means that every item in $A$ has a unique output of $B$, we can get this nice little small theorem

> **Theorem**: Let $A$, $B$ be sets. If there exists an injective $f:A\rightarrow B$, then $|A|\leq|B|$.

### Surjectivity

Think back to my example about the function `getDog` that takes in a person, and returns back the dog that they own. Now suppose you found a dog, is it true that you can always find a person such that this dog will be the output of `getDog`? If that seemed a little obtuse suppose the dog you found is named `Max`. Are you $100\%$ sure that you can find a person $p$ such that `getDog(p)=Max`, or that `Max` has an owner?

Sadly no, that is not guaranteed because sometimes dogs just don't have owners, whether they are a rescue or a stray. This means that while our function takes in people and maps them to dogs, it doesn't map them to *every* dog, there are still dogs missing. We can also say that in this case $codomain(f)\neq im(f)$ (hence why we need the two terms).

What happens though if we say that every dog *was* covered? That would be the idea behind a surjective (onto[^1]) function.

> **Definition**: A function $f:A\rightarrow B$ is **surjective** if $\forall b\in B, \exists a\in A$ such that $f(a)=b$.

As in, no matter what output you select from your codomain, you can always find an input that maps to it. This is equivilent to

> **Theorem**: A function $f$ is surjective iff $codomain(f)=im(f)$.

> **Example**: Let $f:\mathbb{R}\rightarrow\mathbb{R}$, $f(x)=x\cdot\cos(x)$. Prove $f$ is surjective.
{{% callout info %}}
<details>
<summary>Proof</summary>
Suppose we choose some value $y\in\mathbb{R}$ and want to see if we can find some value $y=x\cos(x)$. Note that $x\cos(x)$ is continuous, so by the intermediate value theorem, if I have two points, $f$ will take all values of $y$ between those two points. If you don't remember calculus, just remember that since $f$ is continuous, if I want to get from $a$ to $b$ without lifting my pen, I have to cross every point between $a$ and $b$.
</br>
Remember that $\cos(x)$ is $1$ whenever you are at a value of $x=n\pi$ whenever $n$ is even, and is $-1$ whenever $n$ is odd. So if $y &lt n\pi$, we know that $f(n\pi) = n\pi \cdot 1 > y$
</br>
QED
</details>
{{% /callout %}}

Similar to the previous section, if we know that there are enough items in $A$ to completely cover $B$, then there must be at least as many items in $A$ as in $B$.

> **Theorem**: Let $A$, $B$ be sets. If there exists a surjective $f:A\rightarrow B$, then $|A|\geq|B|$.

### Bijectivity

Bijective functions are super easy if you understand surjective and injective, because its just both!

> **Definition**: A function $f(x)$ is **bijective** iff it is injective and surjective

Bijections can be thought of as a perfect mapping, or relabeling. This means that we can take every item in each of our sets and pair them with each other, without leaving anything out or mixing them up. This makes it pretty clear the sets should be the same size, and by applying the two theorems from previous sections we can see that.

> **Theorem**: Let $A$, $B$ be sets. If there exists a bijective $f:A\rightarrow B$, then $|A|=|B|$.

> **Example**: Let $f:\mathbb{R}\rightarrow\mathbb{R}$, $f(x)=x^3$. Prove $f$ is bijective.
{{% callout info %}}
<details>
<summary>Proof</summary>
To first show that $f$ is injective, we consider what happens if $$f(x)=f(y)\implies x^3=y^3$$. By just taking the cube root of both sides we can easily see that $x=y$ so $f$ must be injective.
</br>
Now to show its surjective, consider some $y\in\mathbb{R}$. If we want to find an $x$ such that $x^3=y$, just set $x=\sqrt[3]{y}$. Thus $f$ is also surjective. Since it is both, $f$ is bijective.
</br>
QED
</details>
{{% /callout %}}

---

## Inverse Functions: Uno Reverso

Bijective functions are actually great to work with because they represent the idea that the two sets can be the same set up to some relabeling. We are pretty much taking every element of our one set and fully transferring it over onto our other set. Because of this, it appears that not only can we perform our function, but we should also be able to *reverse* our function too!

You might remember from middle school that there are these things called inverse functions, that take what $f$ does and "undoes" the operation. So if $f(2)=3$, then the inverse of $3$ is $2$. We can notate this quite simply by saying

> **Definition**: Given a function $f:A\rightarrow B$, $g:B\rightarrow A$ is the *inverse function* of $f$ iff for all $x\in A$, then $g(f(x))=x$. We notate this as $f^{-1}$

Note that a simple definition gives us that if $g=f^{-1}$ for some $f$, then we also have $f=g^{-1}$.

Why am I mentioning inverse functions here though? Sure its cool and whatever but what does it have to do with this lesson? Well it turns out that the following theorem is true.

> **Theorem** <a name="inverse_function_theorem">Inverse Function Theorem</a>: A function $f$ has an inverse iff $f$ is bijective.

Before we do the proof, just take a moment to look at and think about this theorem, because it seems to run counter to things you've done before in classes like algebra and trig. 

---

### Inverse Functions You've Seen Before

You might be thinking "how is it possible that $f$ can only have an inverse if it's bijective? I know a ton of not bijective functions with inverses!"

For example, you might say $f(x)=x^2$ with $f:\mathbb{R}\rightarrow\mathbb{R}$ breaks this rule. Clearly $f$ is not bijective, as $f(1)=f(-1)$ and $1\neq -1$ so its not injective. Also, there are no real numbers you can plug into $f$ in order to get anything negative, so $f$ can't be surjective either! So then how is it that $f^{-1}(x)=\sqrt{x}$ if $f$ is not bijective?

This is because functions can be bijective on specific domains and codomains, think about what you remember the domain and range of $\sqrt{x}$ being back in the day. Well you're not allowed to input negative numbers, and its not possible to get negative numbers as an output. This is because $\sqrt{x}$ is the inverse of the $x^2$ when $x\geq 0$! So being more specific we would say that
$$
    x^2:\\{x | x\in\mathbb{R} \text{ and } x\geq 0\\}\rightarrow \\{x | x\in\mathbb{R} \text{ and } x\geq 0\\}
$$
which makes
$$
    \sqrt{x}:\\{x | x\in\mathbb{R} \text{ and } x\geq 0\\}\rightarrow \\{x | x\in\mathbb{R} \text{ and } x\geq 0\\}.
$$
By extension the negative half of $x^2$ has an inverse of $-\sqrt{x}$, which is why we say $x^2$ has an inverse of $\pm\sqrt{x}$, because there are actually two inverse functions depending on your input domain in which you are injective!

With this we can actually come up with a way to remember all those really confusing domains and codomains for things like trig functions[^2].

> **Example**: Let $f(x)=\sin(x)$. What is the domain and codomain of $f^{-1}(x)=\arcsin(x)$?
{{% callout info %}}
<details>
<summary>Answer</summary>
In order for there to be an inverse function, we need $f(x)$ to be bijective on the whole domain. From there the domain and codomain of $f^{-1}(x)$ are just going to be flipping the input and outputs of $f$. 
</br>
Let's first find a domain for which $\sin$ is injective. We want to make sure there are no two inputs that repeat the same output. As such we can see in a $\sin$ wave that if we start at $0$ and go past $\frac{\pi}{2}$, we will go back down and repeat values. Instead, we can start at $-\frac{\pi}{2}$ and go up to $\frac{\pi}{2}$ which will repeat nothing! This will be our domain, and our codomain will have to take on all the values so that our function will be surjective. $\sin$ goes from $-1$ to $1$ which gives us that $$\sin:\left[-\frac{\pi}{2}, \frac{\pi}{2}\right]\rightarrow [-1,1]$$
</br>
Finally, we have from here that the domain and codomain of $\arcsin$ is $$\arcsin:[-1,1]\rightarrow \left[-\frac{\pi}{2}, \frac{\pi}{2}\right]$$.
</details>
{{% /callout %}}

---

## Bijectivity and Inverses: A Proof

To close out this section, we will prove the **Inverse Function Theorem** shown in the section above. 
{{% callout info %}}
<details>
<summary>Proof</summary>
We first will prove the forward direction, that if a function is bijective then an inverse exists. Suppose $f(x)$ is bijective, then we know that every item in the codomain is uniquely mapped to by some item in the domain. As such we can simply "reverse" $f$ as our way to define $f^{-1}$. This might seem circular, but its enough to show existence
</br>
Now suppose we know an inverse function exists, we can use this to prove $f$ is bijective. First, suppose that $f$ was not injective. This would mean that there could not exist an inverse function as there would be some item of the codomain that $2$ different items mapped to under $f$. How could there be an inverse in that case, as what would the inverse give back?! For example if $f(2)=1$ and $f(3)=1$, then what would $f^{-1}(1)$ be? As such $f$ must be injective.
</br>
Next, imagine $f$ was not surjective. This would mean that there would exist items in the codomain that no input maps to, but this would also imply there is no inverse function, as a function must be defined over the **entire** domain! As such $f$ must be surjective.
</br>
Since $f$ is both injective and surjective, then $f$ is bijective. **Q.E.D.**
</details>
{{% /callout %}}


[^1]: "Onto" is such a dumb fucking term

[^2]: I was never able to remember the domains and ranges of trig functions back in the middle school days, now it makes so much sense
