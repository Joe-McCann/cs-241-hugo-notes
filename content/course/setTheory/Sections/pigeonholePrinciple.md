---
title: Pigeonhole Principle
linktitle: Pigeonhole Principle
toc: true
type: book
date: 2023-05-24
draft: false
tags:
    - cs241
    - functions
    - pigeonhole

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 245
---

## The Hardest Math Theorem ☠️

The pigeonhole principle is probably the theorem in math that scares me more than anything else in the world, because it is so powerful and fundemental, but actually applying it can be some of the most challenging problems a math student can face. This is particularly shocking because the theorem is so crazy easy, that you would almost think there is no point in having it.

I feel that theres likely just a name attached to it so mathematicians don't forget it but who knows.

> Theorem **Pigeonhole Principle**: Suppose you want to place $m$ pigeons into $n$ holes. If you have more pigeons than holes, $m>n$, then at least one hole will have more than one pigeon.

{{% callout info %}}
<details>
<summary>Proof</summary>
Let $P$ be the set of pigeons and $H$ be the set of holes. If $f:P\rightarrow H$ is a function that maps a pigeon to a hole, and $|P|>|H|$ (more pigeons than holes), then $f$ cannot be injective. Since $f$ is not injective there must exist at least one pair $x,y$ such that $x\neq y$ and $f(x)=f(y)$; this means there are at least two pigeons in the same hole.
</br>
QED
</details>
{{% /callout %}}

Why are people putting pigeons into holes? I have no idea, but hey its funny and mathematicians are goofballs.

If we want to make it super hard, we can make the theorem more general, if we were to divide all the pigeons evenly between the holes, we can also say

> Theorem **General Pigeonhole Principle**: Suppose you want to place $m$ pigeons into $n$ holes. At least one hole must have $\lfloor \frac{m}{n} + 1\rfloor$ pigeons.

For example, if I have $10$ pigeons and $4$ holes, then at least one hole must have $3$ pigeons. So fucking hard huh?

### Example Problems

You might be thinking "Joe, how could this shit be hard?" Well here are some example problems that all use the pigeonhole principle. Good luck lmfao.