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

This code will give us the sequence we know and love $0,1,1,2,3,5,8,13,\ldots$. Now lets count some operations to see the runtime of this code. In the event that $n=0$ or $n=1$ we get that $T(n)=2$ by our base case. However, if $n>1$, we find that our operations function has $1$ comparison, $3$ arithmetic, and $2$ function calls. But the two function calls are to the function itself which creates this bizzare situation of
$$
T(n) = T(n-1)+T(n-2)+6.
$$
How can we try to classify this into some $\mathcal{O}$? Can we come up with a clean expression for it?

The answer is **yes**, but we need to learn a new technique.

---

## Recurrance Relations