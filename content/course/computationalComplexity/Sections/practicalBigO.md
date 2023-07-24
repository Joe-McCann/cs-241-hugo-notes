---
title: What Do Software Engineers Mean By Big O?
linktitle: Practical Big O
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

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 310
---

## Hijacked Notation

Like many things in computer science, Big $O$ has a very specific definition and has a use case designed for number theory. Computer Scientists then took that notation, changed nothing about the notation, and then proceeded to use it in whatever way they felt was convenient[^1].

The way that we are going to do this is that I am going to explain to you how computer scientists tend to use the notation, and then I will explain in the next page what the true technical definition is. To make things (hopefully) less confusing, when I refer to the *true tecnical* definition of notation, I will use the superscript $T$. For example, just using Big $O$ in the colloquial computer scientist version I will write $\mathcal{O}(n)$, but when I wanna talk about the technical definition, I will say $\mathcal{O}^T(n)$. **For your problems, you do not need to do this**, however I will do it so it doesn't make shit confusing and keep you guessing about what I mean. Similarly, when I talk about average and ammortized time I'll talk about different notation for that as well.

### \#Ratio'd

Lets look at some operations functions and put them into a table
$$
$$\begin{array}{c|c|c|c|c}
n & 6n-5 & n+20 & n^2-n+2 & 8n^2+6 \\\\ \hline
1 & 1 & 21 & 2 & 14 \\\\
10 & 55 & 30 & 92 & 806 \\\\
100 & 595 & 120 & 9902 & 80006 \\\\
1000 & 5995 & 1020 & 999002 & 8000006
\end{array}$$
$$

It might not be immediately apparent that there is a pattern here (apart from just plugging shit into a function), but we can make an observation. Lets take the columns of $6n-5$ and $n+20$ and take the ratio of these two functions as $n$ gets bigger and bigger. This might seem kind of random, but it has a real application in that we can use it to compare the two in terms of runtimes. Lets look
$$
\begin{align}
\frac{1}{21} &= .0476\ldots \\\\
\frac{55}{30} &= 1.8333\ldots \\\\
\frac{595}{120} &= 4.9583\ldots \\\\
\frac{5995}{1020} &= 5.87745\ldots \\\\
\end{align}
$$
and if we were to continue this, we would find that our ratio is getting closer and closer to $6$ as $n$ gets bigger and bigger! In a practical sense, we would then say that the code with operations function $6n-5$ should take about $6$ times as long to run as the code that takes $n+20$ operations. So as $n$ grows larger, even though the $6n-5$ function will take longer, we know that it will remain within a ratio no matter how big our input gets. 

Similarly, if we check $n^2-n+2$ and $8n^2+6$ we can see that
$$
\begin{align}
\frac{14}{2} &= 7 \\\\
\frac{806}{92} &= 8.7608\ldots \\\\
\frac{80006}{9902} &= 8.079\ldots \\\\
\frac{8000006}{999002} &= 8.0079\ldots \\\\
\end{align}
$$
in this case the ratio appears to be getting closer and closer to $8$!

However, lets consider the ratio between $n+20$ and $8n^2+6$.
$$
\begin{align}
\frac{14}{21} &= .666 \\\\
\frac{806}{30} &= 26.866\ldots \\\\
\frac{80006}{120} &= 666.7166\ldots \\\\
\frac{8000006}{1020} &= 7843.1431\ldots \\\\
\end{align}
$$
This is werid, it seems like there is no ratio, and as $n$ keeps getting bigger and bigger that the ratio also gets bigger and bigger. This shows us that even as our input grows larger $n+20$ will take longer to run, but the amount of time $8n^2+6$ takes to run is so big in comparison that we can't even find a number to relate the two of them!!

As such, we could almost imagine $6n-5$ and $n+20$ are in some sort of a "class" of functions that all share this common ratio, while $n^2-n+2$ and $8n^2+6$ would be in a different class.

This is the general idea of a software engineer's version of Big $O$, we are trying to group the functions together into classes that share common ratios. 

{{% callout warning %}}
Notice that we are not saying anything about which function at any point is the fastest, just relating ratios as $n$ gets larger and larger. In fact, when $n=1$ the function $n+20$ ran the slowest! As such you shouldn't make any speed claims about specific inputs based solely on Big $O$ information.
{{% /callout %}}

---

## The Computer Scientist's Playbook

For the following sections, we will use this information for our problems, just so we don't need to keep referencing it over and over and over again. Let $B(n)\leq T(n)\leq W(n)$ be strictly positive operations functions, with $B(n),W(n)$ being the best case and worst case functions. Let $n$ represent the "size" of a given input[^2].

Remember, these are not the true technical definitions, rather a way that computer scientists and software engineers generally use them.

### The Biggest of $O$s ⭕



[^1]: No I'm totally not salty.

[^2]: Astute readers would realize that $T(n)$ actually isn't a function cause the same size inputs could have multiple outputs, but we're going to not go too hard with this.