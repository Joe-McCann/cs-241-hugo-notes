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

### If-Else Examples 🌴

Consider the following piece of code

```python
def make_even(x):
    if x % 2 == 0:
        return x
    else:
        return x+1
```

In the event $x$ is divisible by $2$ which makes $x\%2=0$, then we would have $1$ comparison, $1$ arithmetic with $\%$, and $1$ return giving us $T(n)=3$ operations. However, in the event our number is not divisible by $2$, then we have all of that with an extra addition giving us $T(n)=4$ operations! So which one do we pick, $3$ or $4$?

For most algorithms, we won't have a definitive single formula that defines exactly how many operations will occur because how the program behaves is often so incredibly dependent on the input. Most of the time we care specifically about the *worst* case and the *best* case, because then we know exactly the window of operations all of our code will run[^3].

> **Definition**: Let $T(n)$ be an operations function. The *worst case* $W(n)$ is the "smallest" function such that $T(n)\leq W(n)$ for all possible inputs. The *best case* $B(n)$ is the "largest" function such that $B(n)\leq T(n)$ for all possible inputs.

Note that I put the words "largest" and "smallest" in quotes, because those are definitely not mathematically rigorous definitions lmao. I use this because obviously $B(n)=0$ and something like $W(n)=n^{n^n}$ will be smaller and bigger (respectively) than any algorithm we think up, but these will not be useful for any discussion at all. *"No shit dude, I know your code runs slower than $0$, thats code that runs nothing"*. So for us, we will choose $B(n)$ and $W(n)$ to be functions that are the closest bounds we can represent[^4].

In our example of `make_even`, we know that at best we run $3$ operations, and at worst we run $4$ operations. From this we can say that $B(n)=3$ and $W(n)=4$. Lets show a more practical example that will have a more complicated best and worst case.

```python
def maximum(arr):
    # Given a non-empty list of numbers, find the largest
    largest = arr[0]
    length = len(arr)
    i = 1
    while i < length:
        if arr[i] > largest:
            largest = arr[i]
        i = i + 1
    
    return largest
```

Let $n$ represent the length of the list. Also, we will say that `len(arr)` costs $1$ operation for the purposes of counting; clearly this isn't true, as it has to call the function, return, and actually compute the answer, but this margin of error is small enough to not matter to us. 

Outside of the loop, we have $3$ assignment, $1$ array indexing, $1$ `len(arr)` call, and $1$ return which gives us $6$ operations. Now inside the loop, you can see that how many times the code inside of the if statement actually depends on the placement of the numbers inside of the array, so for us we are only going to try to find the *best* and *worst* case. These will be the cases in which we run the least number of operations possible, and the most operations possible.

The best case is if we never actually run whats inside of the `if` statement; this happens when `arr[0]` is the maximum. In that case we have $2$ comparisons, $1$ assignment, $1$ addition, and $1$ indexing. This gives us a total of $5$ operations that loop $n-1$ times since we start at `i=1`. With the $1$ final comparison we get our best case that
$$
B(n)=6+(n-1)(5)+1=5n+2.
$$
For our worst case, this would happen if we ran the `if` statement every single time; this occurs if the array is in ascending sorted order. In this case our worst case is the same as the best case, however we run `largest=arr[i]` each loop wich is $2$ operations. This means
$$
W(n)=5n+2+(n-1)(2)=7n.
$$
Putting this together, we can show that for any list of size $n\geq 1$, the number of operations $T(n)$ is in the bounds
$$
5n+2\leq T(n)\leq 7n.
$$

Just a note though, the worst case scenario is incredibly unlikely for a random array, with probability $\frac{1}{n!}$, where the best case is much more frequent with probability $\frac{1}{n}$ (or better if duplicates exist). This motivates the idea of an "average case", but we'll discuss this later.

---

## Counting the Operations of Bad Sorting Algorithms

Before moving on to defining what $O$ is, lets take a moment to look at a practical application of counting operations for the shitty $O(n^2)$ sorting algorithms you first learn about in intro coding classes. The three algorithms in particular are *insertion sort*, *selection sort*, and *bubble sort*, which you might have learned as "the confusing one", "the easy one", and "the garbage one" (at least thats how I felt).

If you run these $3$ algorithms against each other though (without optimizations just to make it simpler), you'll find that *bubble sort*, even though it has the same Big-O as the other algorithms, is just *waaaaaaay* slower than the other two. What the hell is going on to make this the case?? Let's observe and count the operations of these functions to see whats happening here.

### Insertion Sort

I hate this sort

[^1]: Have fun in CS$341$ 😂
[^2]: These operations were given to me by a Facebook Engineer I used to TA, and I've found them to be a useful approximation. Here is a stack exchange link discussing their merits: https://cs.stackexchange.com/q/160969/81348.
[^3]: ACtually a ton of people only care about worst case but best case is also fine.
[^4]: Note that sometimes you might not be able to actually explicitely write out $B(n)$ or $W(n)$, we will get into the notation we use that addresses this later.
