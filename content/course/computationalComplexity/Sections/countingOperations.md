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

This is pretty straightforward, as inside of our function we have $1$ assignment, $1$ aritmetic with addition, and $1$ return. As such, the total number of operations is as follows
$$
T(n) = 1+1+1 = 3.
$$
Woah woah there, what the fuck is that $T(n)$? We were just counting operations, how'd we get a function here? Don't worry I'll explain that eventually but for now we will stick with just putting it there with the number of operations. So running the code inside of `plus_two`, we would find ourselves with $3$ operations. Let's show another example.

```python
def square_plus_two(x):
    y = x * plus_two(x)
    return y
```

In this case we have $1$ arithmetic, $1$ assignment, $1$ return, and $1$ function call with `plus_two`. However, when we run `plus_two`, we have to include the operations that are run inside of that function too which is $3$! As such we find that we have
$$
T(n) = 1+1+1+1+3 = 7.
$$

---

### Looping Examples 🔁

So far these are very simple examples, but lets discuss our first control structure: the loop. 
{{% callout info %}}
For the purposes of counting operations, we are going to use `while` loops, as they are easier in `python` to see whats going on compared to a `for` loop.
{{% /callout %}}

Lets consider a simple loop that iterates a fixed number of times.

```python
def add_10(x):
    i = 0
    while i < 10:
        x = x + 1
        i = i + 1
    
    return x
```

Outside of the loop, we have $1$ assignment and $1$ return. Lets consider the loop, inside we have $2$ additions and $2$ assignments and we also have that we compare `i < 10` every time we iterate. This gives us $5$ operations that are run every loop. Finally, there is $1$ extra comparison that is run at the end when `i=10` to actually break out of the loop. Since we run the loop $10$ times, we can multiply our $5$ operations per loop by that to get us that 
$$
T(n) = 2+10\cdots (5) + 1 =53.
$$

Remember the extra $1$ at the end is for the final comparison. We're starting to get larger amounts of operations, but just wait cause you haven't seen nothin' yet. Lets consider the following interesting function

```python
def add_n(x, n):
    i = 0
    while i < n:
        x = x + 1
        i = i + 1
    
    return x
```

Well, ok this isn't really all that interesting of a function because it seems to be totally pointless, however whats interesting is how many operations we have, because its all the same as the previous function except instead of looping for $10$ times, we loop for some unknown amount $n$ times! We can use this as a variable in our function to get something that we can "plug in" the value of $n$ into!
$$
T(n) = 2+n\cdot(5)+1= 5n+3
$$
where $n$ denotes the numerical value of the parameter $n$. Notice that from this, if we set $n=10$, we get back our $T(10)=53$ from the previous problem, and nothing is stopping us from trying out other values of $n$ as well if we wanted.

Before we move to the next control structure, lets consider an example of a nested loop.
```python
def square(n):
    # Look at this horrible way to input n and get n^2 !
    i = 0
    total = 0

    while i < n:
        j = 0
        while j < n:
            total = total + 1
            j = j + 1
        i = i + 1
    
    return total
```

Lets start by counting the operations that are outside the loops, of which we have $2$ assignments and $1$ return. Now lets consider the outer loop; this loop contains the comparison `i<n`, the assignment `j=0` and $2$ operations in `i=i+1`, while also having a final comparison of the inner loop. We see though that every time the outer loop runs, so does the inner loop though, so however many operations the inner loop runs, we *also* need to multiply that by $n$. Our inner loop has $1$ comparison, $2$ assignments, and $2$ arithmetic.

Holy fuck that was a lot, but if we write it all out we can find that
$$
T(n) = 3 + n\cdot\left[1+1+2+n\cdot\left(1+2+2\right)+1\right] + 1 = 5n^2+5n+4
$$
where $n$ represents the numerical value of the parameter $n$. This function grows quite fast, as plugging in for $T(10)=554$ which is a much larger number than we've seen so far. At $T(100)=50504$, which might give a scale that things are gonna get pretty fast pretty soon.

### If-Else Examples

Consider the following piece of code

```python
def divide_two(x):
    if x % 2 == 0:
        return x/2
    else:
        return x
```

[^1]: Have fun in CS$341$ 😂
[^2]: These operations were given to me by a Facebook Engineer I used to TA, and I've found them to be a useful approximation. Here is a stack exchange link discussing their merits: https://cs.stackexchange.com/q/160969/81348
