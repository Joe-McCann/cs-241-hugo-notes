---
title: The Well Ordering Principle
linktitle: Well Ordering
toc: true
type: book
date: 2023-01-24
draft: false
tags:
    - cs241
    - set theory
    - induction

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 225
---

## Smallest Numbers

Consider the natural numbers $\mathbb{N}$, what is the smallest number? Clearly the answer is $0$, as it is smaller than every positive number, and there are no negative numbers in $\mathbb{N}$ so it must have $0$ as the smallest. 

Now consider $A=\{1,2,3\}\subset\mathbb{N}$, does this subset have a smallest number? This isn't a trick question, the answer is pretty clearly $1$. 

What about the set of all even numbers $\mathbb{E}=\{0,2,4,\ldots\}$?. Again this is pretty straightforward to see the answer is $0$. This is pretty simple, but it leads to a very important theorem that proves to be quite useful in theoretical mathematics.

> **Well Ordering Principle**: Every non-empty subset of the natural numbers $A$ contains a smallest element.

{{% callout info %}}
<details>
  <summary>Proof</summary>
  We will do this using a proof both by induction and contradiction. Assume that there exists some set $A\subset\mathbb{N}$ where $A\neq\varnothing$ and $A$ does not have a smallest item. If $A$ does not have a smallest item, then $0\not\in A$ because $0$ is the smallest possible natural number, so if $0$ was inside $A$ then it would automatically be the smallest item.
  </br>
  Now we will show that $A$ must actually be empty by induction. Assume that for some value $n$, every natural number $< n$ is not in $A$, so
  $$
  \begin{align}
    0&\not\in A \\\\
    1&\not\in A \\\\
    &\vdots \\\\
    n-1&\not\in A
  \end{align}$$
  since none of the values are in $A$, if $n\in A$ that would make $n$ the smallest item. But since $A$ has no smallest item we know that means $n\not\in A$. Continuing our argument we know by induction that $n+1, n+2, n+3\ldots\not\in A$ and since we have our base case $0\not\in A$, we know this means that for every $x\in\mathbb{N}$, $x\not\in A$. This means that $A=\varnothing$ which contradicts our assumption $A$ was non-empty and proves our claim
  </br>
  <b>Q.E.D.</b> 
</details>
{{% /callout %}}

It turns out you can use the Well Ordering Principle to prove some properties of sets by considering the set of all conter examples, and finding a contradiction involving the smallest element of counterexample.

