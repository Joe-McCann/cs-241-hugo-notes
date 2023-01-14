---
title: When Am I Ever Going to Use This?
linktitle: Practical Applications and Proof Advice
toc: true
type: book
date: 2023-01-07
draft: false
tags:
    - cs241
    - logic
    - proofs
    - coding interviews

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 230
---

## Proofs Seem so Hard, How do I get Better at Writing Them?

Before we get in to the practical application of logic and proofs to your (a CS student's) career, we will first make a detour to answer a question that I often get when talking about proofs to students who are unfamiliar with them.
*"When you go over proofs, the answer makes sense, its just that I have a hard time seeing what steps I should take when I do it myself"*

This is a totally fair question, as proofs are usually completely unlike most types of mathematics a standard student has seen in their college career to this point. Usually you just see a problem like
$$\int\frac{x^2}{\sqrt{25+x^2}}dx$$
that you have to solve and say, *"alright, integral with square root on the bottom, probably has to do with trig subs"*, from which you just grind through the problem the same way as you always do. Even if a particular problem is hard, the worst you'll do is just plug it in to Symbolab and say *"Ah shit it was a by-parts problem"*[^1]. Solving proofs is actually much of the same process, except instead of calcuations its arguments, and instead of Symbolab its Math StackExchange!

Of course the best, and most unhelpful, advice on getting better at proofs is to *practice*, which I know to most people is an idea worse than death lmfao 💀.

---

### What Do I Actually Do Though?

Here is a good step by step proof-writing process that I like to follow whenever I tackle a new problem. Feel free to adjust this process to however you feel best fits you and your style of thinking though. Suppose we wanted to prove the following theorem

> __Theorem__: Let $n>2$, if $n$ is prime then $n$ is odd.[^3]

We haven't gotten into a discussion of primes, even, odd, but I hope that these are enough of grade school topics that I can discuss them at a surface level at least to show how we'd solve the problem.

#### Step 1: Gather Information

Like all problem solving processes, the first step should always be one of gathering information so that you better understand not just what you are trying to solve, but what tools you have at your disposal. Like a lot of fields, mathematics likes to be as general as possible, so any information provided to you in a problem is information that you can use at your disposal. Should a piece of information not be necessary, then it would not be included in the problem and restriction! For real, what would be the point of saying $n>2$ if we didn't even need that to be true, we'd be limiting ourselves for no reason. With all that said, lets see what info we can gather from this problem.

1. $n>2$
2. $n$ is prime
3. $n$ is odd

#### Step 2: Try Some Examples

Now that you understand the problem, its good to try some examples to prove to yourself that the statement you are proving is in fact true (or false). __Remember that doing out examples is not enough to prove the statement__[^2], however they are goo ways to get grips on problems and get a good understanding of what may be going on. For our problem we might observe some primes $>2$ and see we have
$$3,5,7,11,13,17,19,23,\ldots$$
that these are all odd. From here we would think *"ok I can see this true, now whats the underlying mechanism"*

#### Step 3: Use the Methods You Know

Observe the problem and select a way of going about it with a certain technique. As you get more aquainted with certain fields, you'll learn different ways of proving things, which you can then add to your toolbox. However, without fancy methods like those, direct proof, proof by contradiction, and induction will always be able to do the trick (with a little more effort). Leverage the things that you already know how to do and decide on a direction.

Similarly, see if this problem appears familiar to one that you've already solved, cause in that case there is a high probability that you will be able to use a similar method.

#### Step 4: Grind, Fail, and Repeat

Once you've established a method and know what info you are solving for, it comes to the time that is the most frustrating: the grind section. This is the section where you sit down and attempt to tackle the problem, and also why this tutorial might seem unhelpful. *"Obviously Joe I understand that I need to select a method, thats not the hard part, the hard part is knowing what to do"*. Once you have the method and information though, you can take all of that and being to "play" with the problem, which I also like to call "doodling". This is where you start moving around in different tangents using the information you know until eventually you get to a spot that solves the problem.

Maybe for this problem you first start by looking at odd numbers and even numbers. What makes a number odd? Are all odd numbers prime? No they're not, ok how about some other direction. Why do we need $n>2$? Does that matter? Ok $2$ is prime but why are no other even numbers prime? Oh even numbers are divisible by $2$!

Boom just like that you have an outline of a proof, and all you gotta do is make it tight so that your language doesn't leave way for holes.

Obviously, if you find yourself stuck and hitting a wall, it might be time to go back to step 3 and try another method. Keep grinding and repeating until you get it!

## How Can I Use This in CS Though 💻?

[^1]: Perhaps this is why people feel mathematics is so boring, as most problems are the same 5 step processes that you repeat and bang out until you've reached some solution. Musings about how education fails to explore the creativity in mathematics is a discussion for another day.

[^2]: Unless you're dealing with providing a counter-example then you only need one

[^3]: We prove this theorem in another section of this website.
