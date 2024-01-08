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

> **Minimum Distinct Items Problem**: Given a list of $n$ integers[^1] `arr` and an integer $M \geq 0$, provide the minimum possible amount of unique items after removing at most $M$ items from the list.

Let's run through some examples

> **Example**: `arr=[1,1,4,6,7]`, $M=2$

{{% callout info %}}
<details>
<summary>Answer</summary>
In this case we could remove any two of $4,6,7$ to get our answer. If I remove $4,6$ then our array would be `arr=[1,1,7]` which has two unique numbers and is correct.
</details>
{{% /callout %}} 

Now before we get into the solutions, there is an important thing to notice that will help with our algorithm. Hopefully this is obvious to notice, but the optimal solution is going to use the maximum amount of removals possible. We're going to prove this, but that should make sense because if we don't use every removal, we could either do better or the same by using those extra ones.

> **Lemma**: If $n \leq M$ the optimal solution is to remove every item. Otherwise there is an optimal solution that will use all $M$ removals.
{{% callout tip %}}
<details>
<summary>Proof</summary>
For the first part, if $n\leq M$ then we can remove every item which will provide $0$ unique items. This must be the optimal solution in this case.
</br>
In the event $n > M$, assume that an optimal solution only removes $m < M$ items, and there is not a solution that removes the maximum amount. However, if we remove the remaining $M-m$ items it will at worst remove the same amount of unique items as the $m$ solution, meaning that we have an optimal solution that removes $M$ items, contradicting our previous assumption.
</br>
<Q.E.D.>
</details>
{{% /callout %}} 

### A Brute Force Solution

As with all coding interview problems, its always a good idea to go through the brute force solution at least to talk through. In this case, the brute force solution is quite bad, so I won't code it since its not worth it.

In order to brute force, we can iterate through all possible selections of $M$ numbers to remove, and see which one is the best. This would require checking all $M$ item combinations from our list which would mean that our algorithm would have to check $\binom{n}{M}$ options. As $n$ grows large this means that our algorithm would be $\Omega^T\left(n^M\right)$[^2] and for values of $M$ that can be big this just isn't an option.

We wouldn't need to prove this solution though, as anything returned is guaranteed to be optimal by definition.

Writing this code out anyway, we will use the itertools in Python to provide all combinations of the items, and then will check how many unique items exist in that new list. Note that $\binom{n}{M}=\binom{n}{n-M}$ so it doesn't matter that we are checking how many combinations of non-removed items there are, its all the same.

```python
from itertools import combinations

def unique_items_brute_force(arr, M):
    n = len(arr)
    if n <= M:
        return 0
    elif M == 0:
        return len(set(arr))

    min_unique = len(set(arr))
    for sub_list in combinations(arr, n-M):
        unique_items = len(set(sub_list)) # Theta(n) work
        if unqiue_items < min_unique:
            min_unique = unique_items
    
    return unique_items
```

The amount of work per loop iteration is $\Theta(n)$ and there are $\binom{n}{M}$ loop iterations, which puts this code implementation at $\Theta(n)\cdot\binom{n}{M}\in\Theta(n^{M+1})$.

---

## Getting to the Solution

The solution is pretty straightforward to get to in this problem. The thing about removals is that for each removal, we want to get the most "bang for our buck", which we can achieve by at all points by removing the least common item.



[^1]: List doesn't need to be integers but I put it here just for simplicity
[^2]: $\Omega^T$ represents the "true" mathematicians version of Big Omega that means that means that our implementation would be equivilent or worse to $n^M$.