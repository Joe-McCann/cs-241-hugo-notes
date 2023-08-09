---
title: What Do Mathematicians Mean By Big O?
linktitle: Mathematician's Big O
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

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 315
---

## Why Do Mathematicians Do Something Different?

In case my salty ranting on the previous page couldn't clue you in, I will now take the time to explain what the notation of $\mathcal{O}, \Omega, \Theta$ truly mean in mathematics. Remember that for the purposes of not confusing y'all, I will be denoting the true versions as $\mathcal{O}^T, \Omega^T, \Theta^T$.

The original case was for purposes of analytic number theory, however I think its useful to show it in terms of an example from approximation analysis. Remember back in physics class when your professor said 

> for small enough values of $\theta$, you can use $\sin\theta\approx\theta$?

Well lets see how good of an approximation this is we can provide the Taylor Expansion of $\sin\theta$ (don't worry if you don't know where this comes from, just take it on faith here) to show that the error of the approximation for any value of $\theta$ is
$$
\begin{align}
\text{Error}(\theta)&=\theta - \sin\theta \\\\
&= \theta - \sum_{n=0}^\infty\frac{(-1)^{n+1}(\theta)^{2n+1}}{(2n+1)!}\\\\
&= \theta - \left[\theta - \frac{\theta^3}{3!}+\frac{\theta^5}{5!}-\frac{\theta^7}{7!}+\ldots\right] \\\\
&= \frac{\theta^3}{3!}-\frac{\theta^5}{5!}+\frac{\theta^7}{7!}-\ldots
\end{align}
$$

Great, now we have an expression for our error! Plugging this expression into a calculator though would take an infinite amount of time though, so we really just want to get a good sense for how this error changes for various values of $\theta$. If $\theta<1$, then we can see that raising $\theta$ to higher powers actually makes it smaller, so the biggest term in our error is actually the first one! In fact, we can show that as we make $\theta$ get closer and closer to $0$, our first term of $\frac{x^3}{3!}$ will dominate our expression more and more. We would notate this as
$$
\sin\theta = \theta + \mathcal{O}^T(\theta^3).
$$
In this we are saying that for our limit that we are looking at, that our function effectively looks like $\theta$ with some extra term that is kinda like $\theta^3$.

That was probably really confusing so lets actually get to the definition.

---

### The Biggest of True Os

The idea of Big $O$ is that we want to be able to answer the question *"I have some function $f(n)$, will it eventually be smaller than this other function $M\cdot g(n)$ where $M$ is a constant?"*. The reason why we are asking is because we want to be able to provide ourselves with some worst case rate for how we are growing over time so that we can say "ok yeah this is good enough".

This is actually going to be best explained using examples and the definition, so I will do that so we can move into it.

> **Definition**: We say a function $f(n)\in\mathcal{O}^T(g(n))$, also notated $f(n)=\mathcal{O}^T(g(n))$ iff there exists an $N>0$ and $M>0$ such that $\forall n > N$, $f(n)\leq M\cdot g(n)$.

What this definition is saying is that if we multiply our function $g(n)$ by some constant of **our choice** $M$, we can find a value of $N$ such that for any value of $n$ that we choose past that, our function times $M$ will be larger than $f(n)$.

Lets first show an example where we don't need $M$ so we can get a good idea of whats going on here. In later examples, choices of $M$ will actually make proofs easier though.

> **Example**: Prove $\frac{1}{2}n^2 + 5n + 10\in\mathcal{O}^T(n^2)$.
{{% callout info %}}
<details>
<summary>Proof</summary>
We can directly find our value of $N$ by solving the inequality
$$\begin{align}
\frac{1}{2}n^2 + 5n + 10 &\leq n^2 \\
0 &\leq \frac{1}{2}n^2 -5n -10
\end{align}$$
We can solve this using the quadratic formula to find our positive root is $N=5+3\sqrt{5}\approx 11.71$. As such, so long as $n\geq 12$, we can see that $n^2$ will be bigger than $\frac{1}{2}n^2 + 5n + 10$, which means that our claim is true!
</br>
QED
</details>
{{% /callout %}}
Quite obviously, this doesn't really give a super good approximation of any meaningful rate of error, but it shows us that if $n^2$ is considered "good enough" for us, well then the full expression will too, and we don't need to bother our efforts working on it.

But then why do we need to have the factor of $M$ be considered at all? Can't we just always do this? Well not always, consider the example below.

> **Example**: Prove $2n\in\mathcal{O}^T(n)$

Well these two functions grow at the same rate, so it would be nice if we could do the same thing as before and handwave to say "yeah its just $\mathcal{O}^T(n)$ no worries", but in this case $2n\not\leq n$ for any value of $n>0$, so we need to use our value of $M$. If we set $M=3$ for example, then we can say that $2n\leq 3n$ for every $n>0$, which would prove us true. Note though, that our choice of $M$ was not unique, and we could have made infinite selections. In fact, sometimes a choice of $M$ can depend on your choice of $N$!

> **Example**: Prove $2n^2+2n-10\in\mathcal{O}(n^2)$
{{% callout info %}}
<details>
<summary>Proof</summary>
In order to prove this we cannot do the same method before, as $2n^2+2n-10\geq n^2$ for large values of $n$. Instead we can apply a super clever trick. First lets factor out $n^2$ from our $2n^2+2n-10$ to get $$n^2\left(2+\frac{2}{n}-\frac{10}{n^2}\right).$$ Now something interesting to notice is that $\frac{2}{n}\leq 2$ if $n\geq 1$ and $-\frac{10}{n^2} < 0$ since its negative. As such we can say that if we choose $N=1$, for any value of $n\geq 1$ we can see $$2+\frac{2}{n}-\frac{10}{n^2} < 2+2=4.$$ Because of this we can say that if $n\geq 1$ then $$2n^2+2n-10 \leq 4n^2$$ which proves our claim.
</br>
QED
</details>
{{% /callout %}}

Now notice though, if we didn't really care about the smaller values of $n$, we could see that if $n\geq 1000$ then we would have that $\frac{2}{n}\leq .002$, and by extension
$$
2n^2+2n-10\leq 2.002n^2
$$
for values of $n\geq 1000$. These values of $M$ are the *constant factors*, and we can see that if we get larger and larger values of $N$ selected, then our values get closer and closer to $2$.

---

### The Biggest of True $\Omega$s and $\Theta$s

The true definition of Big $\Omega^T$ is actually exactly the same as $\mathcal{O}^T$, except now we use $\geq$ instead of $\leq$.

> **Definition**: We say a function $f(n)\in\Omega^T(g(n))$, also notated $f(n)=\Omega^T(g(n))$ iff there exists an $N>0$ and $M>0$ such that $\forall n > N$, $f(n)\geq M\cdot g(n)$.

This is saying that, if we find a large enough value of $N$, eventually $g(n)$ will be smaller than $f(n)$. Since we did previous examples with $\mathcal{O}^T$ already, and its super late and I need to shower and I have work tomorrow, I think Imma just leave it here for y'all to try your own examples for now.

> **Definition**: A function $f(n)\in\Theta^T(g(n))$ iff $f(n)\in\mathcal{O}^T(g(n))$ and $f(n)\in\Omega^T(g(n))$. 

So, same definition as before, just using the true versions instead.

## Practice Problems

> **Problem**: Prove that $\mathcal{O}^T$ is **not** an equivalence relation. This is the biggest difference in the two versions!
