---
title: What Do Mathematicians Mean By Big O?
linktitle: Practical Big O
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

### The Biggest of True Os

### The Biggest of True $\Omega$s

