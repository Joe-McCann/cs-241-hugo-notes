---
title: Counting like We're Kids Again
linktitle: Counting Operations
toc: true
type: book
date: 2023-07-03
draft: false
tags:
    - cs241
    - software engineering
    - computational complexity
    - asymptotics

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 305
---

## What are we Counting? 🍎🍊🍐

So if we have two pieces of code, how do we know which piece of code will be "faster"? Yeah sure we can run the two of them side by side, but how fast code runs depends on a lot of factors, such as what device it was run on and what other programs were running at the same time. Also, certain pieces of code will be better or worse for different cases, or different sizes of inputs.

If I run a version of code on my laptop for a list of $10000$ items, and you run a version on a Windows $97$ machine with a list of $100$ items, there is a very solid chance that my laptop is going to win just by being better hardware. Also my code will be 🔥 and yours garbage so that also wouldn't help you.

What we are going to do is define one "unit" of code processing, and call that an *operation*. Now in order to see what code should be faster, all we gotta do is see which one performs less operations and we're all good! This should seem straightforward enough, but alas as always, things are never as easy as they appear.

### Ok, but like, what *really* is an operation?

As is tradition, different people do things differently, so they have different definitions to fit their use cases to make their lives easier. If we were going off the *purely theoretical* definition, we would say that
$$
1\text{ Operation}=1\text{ Turing Machine Iteration}.
$$
However, we normal everyday software engineers are not insane and do not build Turing Machines from scratch; also y'all (probably) don't even know what a Turing Machine even is[^1]. As such we need a more practical way of doing this.

Well, we can get more practical, as all code that's run on computers is actually exectued machine code. So we could say that
$$
1\text{ Operation}=1\text{ Machine Code Instruction}.
$$
Still though, we are not crazy, and do not actually code in machine code. For our purposes we need something that's *actually* tangible to count when you are looking at how many operations are inside the code you write for class or work. As such, I am going to give you a list of $7$ types of instructions that can provide you with a high level enough insight for languages like *Python*, *Java*, *C++*, etc.[^2]. These are what we will define as an *operation* for the purposes of this class.

> **Definition**: For a high level language, an **operation** is any of the following instructions:
> 1. Arithmetic ($\*, +, -, /$)
> 2. Logical Operators (and, or, bitshift, not)
> 3. Comparisons ($<, >, \leq, \geq, ==, \neq$)
> 4. Variable Assignment
> 5. Calling a Function
> 6. Returning from a function
> 7. Pointer Derefencing / Array Indexing / Object Access

Its important to know that people in industry don't *actually* go through and count the number of operations they compute, and instead they just freehand it. However, counting operations by hand will help give you a good sense of what you algorithm's runtime is going to look like so you can eyeball it in the future.

### Other Examples of Operations

Before moving into examples of counting operations in code, I am going to provide some other examples of ways you can define operations to fit your needs. 

For example, when mathematicians write code that performs numerical algorithms, they generally don't really care about things like loops or logical control structures. All their code is very straightforward, and nearly all the time is spent actually performing intenvsive math. For them, they define operations through how many "FLOps" (Floating Point Operations) they have
$$
1\text{ Operation}=1\text{ Floating Point Operation}.
$$

For people doing database work, often the methods that call the databases are the costliest, so they only count an operation as a database call. Similarly, for comparison based sorting algorithms, sometimes people will base their operations off of how many swaps they perform.

---

## Examples of Counting Operations ➕

In this section, we will start with some simple cases of counting operations, and eventually build up to some harder scenarios.

### Simple Examples

> **Example**: Count the operations in the following function `plus_two`

```python
def plus_two(x):
    y = x + 2
    return y
```

[^1]: Have fun in CS$341$ 😂
[^2]: These operations were given to me by a Facebook Engineer I used to TA, and I've found them to be a useful approximation. Here is a stack exchange link discussing their merits: https://cs.stackexchange.com/q/160969/81348
