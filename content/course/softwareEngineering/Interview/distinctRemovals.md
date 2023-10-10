---
title: Minimum Distinct Items in a List After Removals
linktitle: Min Distinct Items After Removals
toc: true
type: book
date: 2023-01-31
draft: false
tags:
    - cs241
    - set theory
    - dictionaries
    - lists
    - code interview

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1201
---

## Problem Description

This problem comes from an interview given to a student of mine (who's name I never can seem to remember), and seemed particularly interesting as it, at first thought, seems to be a good application of sets.

> **Minimum Distinct Items Problem**: Given a list of integers[^1] `arr` and an integer $M \geq 0$, provide the minimum possible amount of unique items after removing at most $M$ items from the list.

Lets run through some examples

> **Example**: `arr=[1,1,4,6,7]`, $M=2$

{{% callout info %}}
<details>
<summary>Answer</summary>
In this case we could remove any two of $4,6,7$ to get our answer. If I remove $4,6$ then our array would be `arr=[1,1,7]` which has two unique numbers and is correct.
</details>
{{% /callout %}} 

### A Brute Force Solution

As with all coding interview problems, its always a good idea to go through the brute force solution at least to talk through. In this case, the brute force solution is quite bad, so I won't code it since its not worth it.

In order to brute force, we can iterate through all possible selections of $M$ numbers to remove, and see which one is the best. This would require checking all $M$ item combinations from our list which would mean that our algorithm would have to check $\binom{n}{M}$ options if $n$ is the length of the array. As $n$ grows large this means that our algorithm would be at best $\Omega^T\left(n^M\right)$[^2] and for values of $M$ that can be big this just isn't an option. As such lets walk through how to think of this.

We wouldn't need to prove this solution though, as anything returned is guaranteed to be optimal by definition.

---

## Getting to the Solution



[^1]: List doesn't need to be integers but I put it here just for simplicity
[^2]: $\Omega^T$ represents the "true" mathematicians version of Big Omega that means that means that our implementation would be equivilent or worse to $n^M$.