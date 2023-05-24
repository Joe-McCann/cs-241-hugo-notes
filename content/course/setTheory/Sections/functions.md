---
title: What is a Function?
linktitle: What is a Function?
toc: true
type: book
date: 2023-01-24
draft: false
tags:
    - cs241
    - functions

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 230
---

## We're Still on Grade School Stuff

Remember back in grade school when the teachers taught you about *functions*? They were always represented as $f(x)$ (which apparently is according to Euler? Idk some YouTube video said its his notation) and were discussed as some sort of "input/output machine". The jist of the idea was that you can input some item into this *thing*, and then it would output something accoridng to some rule. Deadass, this is the same exact way functions are defined in advanced math, just with a bit more mathy talk.

Of course there are a lot of ways that you can take in something and spit something else out, but there are certain rules that you have to follow in order to be considered a function. First, every input needs to have exactly $1$ output. You are allowed to output things like sets or tuples, but if you enter the same input twice, then you must get the same output twice.

Also, if you define your function on some set of inputs, it has to be defined for every value in that set. You can't have the possibility of plugging something in and then going 🤷 *woops division by $0$ my bad*. These give us the definition

> **Definition**: Let $f$ be an object that takes in an item in set $A$ and **maps** it to an item in set $B$, notated as $$f:A\rightarrow B$$. $f$ is a function iff $\forall a\in A$ there is **exactly one** $b\in B$ such that $f(a)=b$.

Some examples of functions are as follows

> **Example 1**: Let $f:\mathbb{R}\rightarrow\mathbb{R}$ be the function $f(x)=x^2$

> **Example 2**: Let $\max:\mathbb{R^n}\rightarrow\mathbb{R}$ be the function that takes in a tuple of numbers and returns the maximum

> **Example 3**: Let $g:\\{1,2,3\\}\rightarrow\mathcal{P}(\\{1,2,3\\})$ be the function that takes in an item and returns a set that contains only that item. ie $g(1)=\\{1\\}$

> **Example 4**: Let $getDog:\mathcal{O}\rightarrow D$ be a function that takes in a dog owner, and returns their dog (assuming everyone has only one dog)

> **Example 5**: Let $getCity:\mathbb{F}\rightarrow C$ be a function that takes in an NFL football team, and returns that team's home city (Teams like Patriots would be Boston).

### Terminology Abound

There are a ton of terms to describe functions, so lets define some. Suppose we have $$f:A\rightarrow B.$$

> **Definition**: The input set $A$ of $f$ is called the **domain**, and the output set is called the **codomain**[^1]

In our previous example $4$, our domain was the set $\mathcal{O}$ of people who own dogs, and our codomain was the set $D$ of all dogs. Note that we cannot select the set of all people as our domain, as some people do not have dogs and our function must be defined for every input in the domain. Should we want to do so, we would have to add an item such as "no dog" to $D$.

But notice that just because we specify something the be the codomain of our function, that does not mean that our function will reach it at any point! In example $1$, our codomain is all real numbers, but if we pick any negative number, there does not exist an $x$ such that $x^2<0$. As such we need a term for the set of all the values our domain can actual get to via the function.

> **Definition**: The **image set** of a function $f:A\rightarrow B$ is the set of all outputs of $f$ from $A$. In set builder $$\im(f)=\\{f(x)|x\in A\\}.

So in example $1$, the codomain is $\mathbb{R}$, and the image set is the set of all non-negative real numbers. You might be wondering *"why did we pick $\mathbb{R}$ as codomain? Could we have picked the image set as codomain?"*, to which the answer is yes! You can set the codomain to anything so long as there are no outputs that point outside of it, as such nothing is stopping you from setting the codomain as the image. Sometimes, its just notationally easier to pick a set that is not the image, or it is unknown what the entire image is.

### Is this Practical for CS?

Its a valid thought to wonder if these rules of functions actually apply to the world of CS, as you might be thinking that there are functions that break the rules laid out here. However I argue this is not the case and functions in CS actually *are* functions in math as well! Here are some explanations to common counter-arguments.

Anything external to the function that changes the manner of the rules can be considered part of the input set, as such if a function uses a global variable, but no parameters, then that is ok. Also, a function that returns nothing as well as performs no side effect is valid as it could output the empty set. Even for random functions, the input seed and current state can be thought of as the input! Code that takes in sensory data, also depends on the exact values of those sensors which would then determine the output.

As such, this function definition holds fairly appropriately.

We also will discuss some other applications of functions, but that will come later!

[^1]: You might have heard this called the **range**, but this is an outdated term.
