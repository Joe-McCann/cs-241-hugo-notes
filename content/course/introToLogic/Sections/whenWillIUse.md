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

---

## How Can I Use This in CS Though 💻?

I've been stringing you along for long enough to finally say exactly how we can use this in an everyday career. The proof solving technique that I described above is literally the general problem solving technique, and in CS its how you tackle algorithm design challenges!

If you'd like something that's a little more tangible, __solving proofs is done the same way as solving coding interview problems__. Proofs are literally problem solving boiled down to the most basic essence, there is no external overhead or frills, rather you just take true things and manipulate them to make more true things[^4]. This is why I like to say to my students that if you get good at proofs, you will get good at solving coding challenge problems.

---

### Example Challenge Problem

Now sure I can just make up an excuse for you to care about my class, but lets actually run through a interview problem and see how we can go about it using the problem solving technique.

> __Example__ Missing Number Problem: Write a function that takes in an unsorted list of $n-1$ unqiue numbers where every number $x$ in the list is $1\leq x\leq n$, and returns the number in the range $1$ to $n$ that is missing from the list.

First thing we need to do when seeing this problem is to observe all information we are provided. First, we can see that our list contains only *unique* numbers, so we will only have $1$ missing number. We do not need to worry about duplicates. Next, we see that our list is *unsorted*, which means that we cannot use fast searching algorithms.

Lets look at some examples of possible given lists
$$
[1,2,4], [3,5,2,1], [6,5,2,4,3,8,7].
$$
Just by observation we can see that $3,4,1$ are the missing numbers. How did you do those problems out when you solved it? Well what coding techniques do we know? We can first try looping, in fact a valid solution is to check if every number is there in the list

```python
def missingNum(arr):
    n = len(arr)
    for i in range(1,n+1):
        missing = True
        for j in range(n):
            if i == arr[j]:
                missing = False
                break
        if missing:
            return i
```

This is a totally valid solution, but it is $\mathcal{O}(n^2)$ in time, so we might want to find a faster way to do it. One might notice that pesky *unsorted* word there, as if sorting the array would give us an idea of where the missing number is. Well of course as we could just iterate through the list until we found the first out of place item, which must be after the missing number! 

```python
def missingNumber(arr):
    arr = sorted(arr)
    n = len(arr)
    for i in range(n):
        if i+1 != arr[i]:
            return i+1

    # if the full list is correct the last number is missing
    return n
```

This is better as this depends on your sorting algorithm, and we know sorting algorithms that can go in $\mathcal{O}(n\log(n))$ time. In fact there are even $\mathcal{O}(n)$ sorting algorithms, if you are working with a fixed range of integers (which we are). So we can get this down even lower. Well, what if instead of sorting, we just went through the array and kept track of all the numbers we see, and at the end if we find a number that isn't there then that's the missing number! Notice that this only works because we have a fixed range of integers.

```python
def missingNumber(arr):
    n = len(arr)
    counts = [0]*n # array of n 0's
    for i in range(n):
        counts[arr[i]-1] = 1 # mark numbers we see as 1's

    for i in range(n):
        if counts[i] == 0:
            return i+1 # first number thats 0 is the missing one
```

This is actually just a simple version of a counting sort, and this has a time complexity of $\mathcal{O}(n)$! This idea of taking a solution, observing what you have, observing what you know, and repeating is also exactly what recruiters want to see! They want to know that you have the ability to think and problem solve

---

### How do I Actually Study Though?

Now I know what you might be thinking *"Joe, this still doesn't really help me prepare for coding interview problems, because I need to know the coding part"*. You are correct, but with this mindset you might save yourself some time, let me explain.

Let me provide an anectdote. When I was a freshmen in college, I used to help my friends Rids, Jules, and Sahiti study for our physics common exams. I liked Physics and was pretty good at it so I helped them through examples they got stuck on while going over past exams. However, I had a very different studying philosophy from my friends.

I believed in knowing the underlying ideas, and then running through one set of practice problems. The ideas were what knowledge was needed to solve the problems, and the one practice set was so that I could make sure I recognized possible exam problems, and caught any areas I might be lacking on. My friends believed in grinding out as many practice exams as possible so they could recognized the problems, and know the material. This often resulted in long study sessions going over the same problem, which was an excuse to spend large amounts of times with my friends so I loved it regardless[^5].

My musings about the past aside, heres how I would suggest going about studying this stuff

1. Practice coding interviews from the *problem solving perspective*. Don't practice and fixate on a specific problem, but rather how you tackle problems you don't know. The odds are in an interview you will not recognize the problem you are given.
2. Practice your fundementals, if you don't understand a tool you will not be able to use it when you need it.
3. Practice problems as a way to see what areas you are lacking, and find patterns in problems. If you see that you struggle with DFS, review that topic. If you struggle with understanding new problems given, practice that instead of solving them.

If you were practicing for a calculus exam, you wouldn't grind through solving $\int x^2dx$ and $\int 3x^2dx$ and $\int 10x^2dx$, no you would do it once to learn how to, and then see that all problems from there are the same. Extrapolate that thinking to interviews!

[^1]: Perhaps this is why people feel mathematics is so boring, as most problems are the same 5 step processes that you repeat and bang out until you've reached some solution. Musings about how education fails to explore the creativity in mathematics is a discussion for another day.

[^2]: Unless you're dealing with providing a counter-example then you only need one

[^3]: We prove this theorem in another section of this website.

[^4]: An argument can be made that the mathematics that you use to prove is the frills, however you can start from some barebone axioms that require no prior knowledge if you'd like to practice simple proof writing.

[^5]: I definitely teased and complained like a lunatic though, Sahiti has a hilarious video of me trying to teach Rids.
