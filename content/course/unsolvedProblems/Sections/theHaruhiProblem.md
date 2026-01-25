---
title: The Haruhi Problem 👧
linktitle: The Haruhi Problem 👧
toc: true
type: book
date: 2026-01-09
draft: false
tags:
    - number theory
    - tiktok
    - haruhi

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5007
---

## Problem Description

How is it possible that genuine mathematical research could be gleamed from the depths of anime fandoms and anonymous message boards? The world of mathematics is bonkers and insights of ingenuity can come from anywhere, even if it takes some time for the genius to be appreciated  

Given $n$ objects or symbols, we know from intro combinatorics that there are $n!$ ways that we can arrange the objects in different permutations, where
$$
n!=\prod_{k=1}^n k = n\cdot(n-1)\cdot\ldots 2\cdot 1 
$$
is the *factorial* function. For this page, we will restrict our "objects" and "characters" to the numbers $1,2,3,\ldots, n$ for the purpose of simplicity. In more math-y terminology we would say that our "alphabet" $\Sigma_n=\\{1,2,3,4,\ldots,n\\}$ and that all finite strings made up of these characters are $\Sigma_n^*$

If we have objects $1,2$ and we wanted to write down all possible permutations, we would see that there are $2!=2$ of them
$$
12,21.
$$

Borrowing some wonderful notation from algebra and group theory, we will define the set of all permutations as follows

>**Definition**: The set of all permutations of the characters $1,2,3,\ldots,n$ is $S_n$

There is a lot of deeper literature and theory related to what permutations are and the set $S_n$, but that is outside the scope of this page. Normally permutations are described as functions, but for us we can just think of them as rearrangements of strings.

Now imagine that I wanted to write a sequence of the characters $1,2$, allowing repeats, where every single permutation can be found within the sequence. For example
$$
s=12221
$$
is a valid sequence, or *string*, because the first two characters cover $12$ and the last two characters cover $21$. For the purpose of notation let us define

> **Definition**: Given a sequence, also known as string, of characters $s$, $s_i$ is the character located at index $i$ and $s_{i:j}$ is the *slice* or *frame*[^2] of characters located from index $i$ to $j$
$$
s_{i:j} = s_is_{i+1}\ldots s_{j-1}s_{j}.
$$

Readers familiar with computer science will recognize this as the familiar notation of the Python slice operator[^1] with the key difference being that in Python the right endpoint is excluded, whereas here it is not. I decided this as I felt it would be less confusing for readers without prior familiarity with Python.

> **Definition**: We say some string $s\in\Sigma_n^*$ **covers** some $p\in S_n$ if and only if you can find some index $i$ such that $s_{i:i+n-1}=p$.

It is a natural question to ask what the shortest string would be that covers all possible permutations for $n$ characters. Specifically, this is called the **superpermutations** problem. 

> **Definition**: A string $s\in\Sigma_n^*$ **covers** all permutations, or is a **superpermutation string**, if for every permutation $p\in S_n$, we can find some index $i$ such that $s_{i:i+n-1}=p$.

In our previous example of $12221$ we can see this string is garbage and sucks because there is a ton of unnecessary $2$s. In fact, we can simply concatenate all permutations together to improve our sequence to only $4$ characters
$$
s=1221.
$$

But even this sucks, because there is no rule that says our permutations cannot overlap! As such, the following string is actually optimal
$$
s = 121.
$$
As we can see, the permutation $12$ is covered by $s_{1:2}$ and $21$ is covered by $s_{2:3}$. Of course, this is not the only optimal string, as we can create another one via relabeling $212$. We can see that this string has a length of $3$ characters, which we will measure with the following function

> **Definition**: Let $L:\Sigma_n^*\rightarrow \mathbb{N}$[^3] be the function that takes a string as an input and returns the length of said string

So $L(123)=3$ and $L(5437548)=7$

I'm now going to commit a cardinal sin here by introducing my own notation, but I think it will be helpful later so 🤷

> **Definition**: Let $\sigma_n\subset\Sigma_n^*$ be the set of all minimal superpermutation strings. 

More formally we would say that if $s\in\Sigma_n^*$ is a superpermutation string, and $s'\in\sigma_n$, then 
$$
L(s')\leq L(s).
$$

Formally placing the problem name into a block, we have our unsolved math addiction machine

> **The Superpermutations Problem**: Given some $n\in\mathbb{Z}_+$, what is $L(s)$ where $s\in\sigma_n$?

I already have it linked in this page, but Greg Egan goes over [here](https://www.gregegan.net/SCIENCE/Superpermutations/Superpermutations.html) into a great amount of detail the state of the art effort on the problem. He does a significantly better job than me. You can also see the current list of known lengths of minimum strings here[^5].

> **Example**: Find an element of $\sigma_3$ knowing that $L(s)=9$ if $s\in\sigma_3$

{{% callout info %}}
<details>
<summary>Answer</summary>
One example that can be found of an optimal string is $s=123121321$. We can see that 
$$
\begin{align}
s_{1:3}&=123 \\
s_{6:8}&=132 \\
s_{5:7}&=213 \\
s_{2:4}&=231 \\
s_{3:5}&=312 \\
s_{7:9}&=321
\end{align}
$$
which covers all permutations.
</details>
{{% /callout %}}

### Simple Theorems Relating to Permutation Strings

Here we will include some basic lemmas and theorems that will we use later on in the page. Feel free to skip them or their proofs should you desire, however for teaching purposes I will use them as warm-up.

I will label these with **SE** to refer to "stupid easy" for when referenced in the future.

The first two are related to what actually constitutes a permutation in our string, and when we can expect to find one. For more casual readers who the language of the proof might get a bit technical, just know that we are proving
1. You can't cover a permutation with some sequence if you are missing a character or contain a duplicate
2. Any sequence of $n$ characters cannot more than one permutation at a time

> **Lemma SE1**: A string $s\in\Sigma_n^*$ covers a permutation if and only if all $n$ characters are present within the string exactly once. 

{{% callout info %}}
<details>
<summary>Proof</summary>
This follows directly from the definition of a permutation as a shuffling of a list of items.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

> **Lemma SE2**: Any sequence of $n$ characters covers either no permutations or exactly one permutation.
{{% callout info %}}
<details>
<summary>Proof</summary>
From <b>Lemma SE1</b> we know that if all characters are not represented exactly once, then there will be $0$ permutations covered. As such, let us now consider what happens when all characters are covered exactly one time, and thus a permutation is covered. 
</br>
Assume that our string $s$ covers at least two distinct permutations $u$ and $v$ where $u\neq v$. However, since $s$ covers the permutations we know that
$$
\begin{align}
u_1=&s_1=v_1 \\
u_2=&s_2=v_2 \\
&\vdots \\
u_n=&s_n=p_n
\end{align}
$$
which means that $u=v$ contradicting our claim that they are two distinct permutations.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

We now have enough to create our first simple bounds

> **Trivial Bounds B1**: Let $s\in\sigma_n$, then $n! \leq L(s) \leq n\cdot n!$
{{% callout info %}}
<details>
<summary>Proof</summary>
For the lower bound, we know that there are $n!$ permutations, and since by <b>SE2</b> we know that no sequence of $n$ characters can cover more than $1$ permutations then we must have at least $1$ character added for each permutation so $n!\leq L(s)$
</br>
For the upper bound, we know that we can create a valid string that covers all permutations by concatenating them all, which would create a string $L(s)\leq n\cdot n!$
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

---

## The Haruhi Problem Backstory

### Original 4chan Thread

The Haruhi problem is actually less a mathematical problem, and more a fascinating story regarding how great mathematical insights can come from non-traditional locations.

I am not an anime nerd, so this following information has been gathered through diffusion. There is an anime called *The Melancholy of Haruhi Suzumiya* or more briefly as *Haruhi*, which in its first season had $14$ episodes. The original broadcasting, however, was not done in chronological order and different DVD releases have been in chronological or broadcast ordering[^4].

In 2011, an user on 4chan's /sci/ board asked the minimum amount of episode viewings it would take in order to watch all possible orderings of the original $14$ episodes. Specifically their image said[^6]

> *Original Poster*: What is the minimum number of Haruhi episodes you would have to watch in order to see the original $14$ episodes in every order possible? 
>
> ***You should be able to solve this***

Outside of the image we have from the post

> *Original Poster*: Ok /sci/, You have a 14 episode series, and you wanna watch it in every order possible. E(n) is the least number of episodes watched to make this possible. For example, if n=3, E=9, because you could watch them in the order 1,2,3,1,2,1,3,2,1 to see all 6 permutations.
>
> Some things I've found about the problem:
>
>E(0)=0
>E(1)=1
>E(2)=3
>E(3)=9
>E(4)=33

Placing the problem from the previous section into the language of the page we have

> **The Haruhi Problem**: What is $L(s)$ if $s\in\sigma_{14}$?

From our trivial bounds **B1** we know that 
$$
\begin{align}
14! \leq &L(s) \leq 14\cdot 14! \\\\
87178291200 \leq &L(s) \leq 1220496076800
\end{align}
$$

It is fascinating that the OP (original poster) correctly determined the length of the minimal superpermutation string for $n\leq 4$, however, it appears that there was likely an earlier thread that led to this problem being discussed, as we can see from this anonymous comment

> *Anonymous Response*: OH FUCK YEAH YOU AGAIN. I'm still working on computer homework though, good luck man.

although later it is implied by OP that this is part of a series of effectively mathematical challenge brain teasers, which could be what this user is referring to.

As is with all internet discussions, including those relating to science and mathematics, the conversation remained professional and collaborative the entire time

> *Anonymous Response*: you didn't even get your own problem. the answer is not 14!, but rather 14*14!, because you are asking for the number of episodes you would need to watch. sage
>
> *OP*: That's only true if you don't allow overlapping. for fucks sake read the damn thread. You were the slow kid in math class weren't you? several anon's understand it and possibly have even solved it for all n.

In this initial thread they came to the belief that the minimal solution was the sum of factorials up to $n$. Unbeknownst to them, this was actually where the current state of the literature was, and through observation they had hypothesized that this was the solution

> *Anonymous Response*: [In response to other user] yeah, it does seem to involve the sum of factorials. But the the thing I can't figure out is why. Also as someone else brought up, how do we prove this is the most efficient method?

It is important to note that this was determined heuristically by them, but not formally proved, however at this point in time there was a known algorithm for generating superpermutation strings called the **Recursive Algorithm**[^2], from Greg Egan's page he describes it as follows

> *Greg Egan*: Given a superpermutation on n–1 symbols, to obtain one with n symbols you perform the following steps:
>
>1. Write out the permutations in the original superpermutation, in the order in which they appear.
>2. Duplicate each of them, placing the new symbol n between the two copies.
>3. Squeeze the result back together again, making use of all available overlaps.

He also then goes on to prove the following theorem

> **Theorem Recursive Bound B2**: Let $s\in\Sigma_n^*$ be a superpermutation string generated by the recursive algorithm, then 
$$
L(s) = \sum_{k=1}^n k! = 1!+2!+\ldots+n!
$$

On his page he provides a proof, however personally I don't feel like writing it down here right now. What this does tell us now though is that we have a new upper bound using **B2**. We now know that if $s\in\sigma_n$ then

$$
n!\leq L(s) \leq \sum_{k=1}^n k!
$$

and thus for the Haruhi Problem
$$
87178291200\leq L(s) \leq 93928268313
$$
which is wonderful as we have now shrunk the bound gap to only $6$ billion. At this point in time though, it was conjectured that the recursive algorithm actually created optimal solutions and was thus an exact solution for the length[^7].

> **Conjecture C1**: If $s\in\sigma_n$ then
$$
L(s) = 1!+2!+\ldots+n!
$$

This would later be proven to be untrue, however this would not be proven false until August 21st, 2014[^8][^9].

### 4chan's Breakthrough

On September 17, 2011 the third thread[^10] in the Haruhi problem series was posted[^11]. We can see from the username and tripcode that this appears to be the same user as the previous posted thread. 

Towards the start of the page, one user goes through a long discussion to come to the following conclusion

> *Anonymous Response*: I think I've proven $n! + (n-1)! + (n-2)! + n - 3$ as a lower bound also, but my reasoning needs to be checked by other eyes

However, the majority of the thread continued to discuss finding exact solutions rather than finding bounds. The thread members discussed a desire to prove the sum of factorials bound **B2** (although they were not basing it on the recursive algorithm) from the OP

> *Original Poster*: Most people, including me thought this was gonna be a snap at first. But 5 minutes into it, I realized there are so many paths and options to account for. Even worse, is that there are multiple efficient sequences. But finding out how many relies on that initial question. I think if we truly understood why the sum of factorials works, we'd know how many options there are. My body isn't ready for that second question though.

This explains why most people did not seem particularly interested in the lower bound proof provided in the sequence of anonymous comments. Similar to the math community, the members of the thread were under the impression that the sum of factorials **B2** was the exact solution. Linked posts in this thread to MathStackExchange[^12] corroborated this perspective. 

Since this provided lower bound is significantly lower than the sum of factorials size, this was taken as a cool, despite not useful, bound. In a blog post in 2013, Nathaniel Johnston actually referenced the thread

> *Nathaniel Johnston*: It is not difficult to improve this lower bound to $n! + (n-1)! + n – 2$ (I won’t provide a proof here, but the idea is to note that when building the superpermutation, you can not add more than n-1 permutations by appending just 1 character each to the string – you eventually have to add 2 or more characters to add a permutation that is not already present). In fact, this argument can be stretched further to show that $n! + (n-1)! + (n-2)! + n – 3$ is a lower bound as well (a rough proof is provided [here](https://mathsci.fandom.com/wiki/The_Haruhi_Problem)[^13]).

Note, Johnston did not link to the original 4chan thread, rather the version that was migrated to the /sci/ wiki page. This migration does imply that the users present in the 4chan thread, despite not providing much commentary on the lower bound, felt it worthwhile to polish, format, and publish to the Wiki page. The page was created by user [Renaldo Moon](https://mathsci.fandom.com/wiki/User:Renaldo_Moon) who also made the majority of contributions early in the page history. One can speculate on this user's involvement in the original thread, as well as the owner of this document [here](https://docs.google.com/document/d/1AXXx1516LLq3wpiIZT_FayLnp6Q7xw8JlmFqsyyoy8Y/edit?tab=t.0) where the proof was typed up, but there is no definitive evidence. 

On August 21st, 2014 Robin Houston published a document to arXiv providing a counterexample for **C1**[^8] when $n=6$ and proving it false for $n\geq 6$. 

This meant that the problem had been blown wide open, and that the 4Chan lower bound could be considered something feasible. However, according to Michael Engen and Vincent Vatter in their 2019 article **Containing All Permutations**[^14] it was not truly recognized in the community as serious

> *Michael Engen and Vincent Vatter*: At least since a 2013 blog post of Johnston, it had been known that there was an argument (on a website devoted to anime) claiming to improve on the lower bound ... However, the argument was far from what most mathematicians would consider a proof, and there had been no efforts to make it into one, in part because the claimed lower bound was so far from what was thought to be the correct answer at the time. However, Egan’s \[2018\] breakthrough quickly inspired several participants of the Superpermutators group to re-examine the argument.

The breakthrough of Egan at described in Engen and Vatter's was to create an algorithm that was able to create superpermutation strings of the following length

> **Theorem Egan Bound B3**: Let $s\in\Sigma_n^*$ be a superpermutation string generated by Egan's Algorithm[^2]. Then
$$
L(s) = n!+(n-1)!+(n-2)!+(n-3)!+n-3
$$

Thus adjusting our bounds of a $s\in\sigma_n$ to now be
$$
n!\leq L(s) \leq n!+(n-1)!+(n-2)!+(n-3)!+n-3
$$

and thus for the Haruhi Problem
$$
87178291200\leq L(s) \leq 93924230411
$$
which shrunk the upper bound by around $4$ million.

This is *finally* when the 4chan proof was formalized and accepted into the cannon of the problem, $7$ years later!

---

## The 4chan Proof

I will now attempt to explain the 4chan proof of the lower bound here, sticking as close to the original argument and terminology as possible. In the paper by Engen and Vatter they "formalize" the proof by modifying the language to fit that of the literature.

However, I personally am slightly irritated by this as the argument of the 4chan proof was not taken seriously for several years by this group due who then had the interesting decision to throw shade on it *"the argument was far from what most mathematicians would consider a proof"*, all before providing an argument that feels less rigorous than the effort performed by the 4chan poster[^15].

As such, I wish to attempt to clean up the 4chan argument and present it in a pretty way so that it can be observed in the way the original anonymous user themselves thought of it. Throughout the proof effort, I will hypothesize about what may have potentially led the user to these conclusions for the sake of teaching.

### Step One

The proof follows a straightforward structure, we will imagine that we have some superpermutation string $s$ and then look at increasing sequences of characters. As we increase and add more characters to the subsequence that we are looking at, we will be keeping track of some counters, and will then show that those counters must always be smaller than the length of $s$. This will help us bound the string size.

We will start with the simplest bound, which is just a slightly improved version of **B1**. Steps one and two will be used almost as stepping stones to get us to the eventual desired bound, so I will not label them. 

> **Lemma Bound B4**: Let $s\in\Sigma_n^*$ be a superpermuation string, then
$$
L(s) \geq n! + n - 1
$$

We will first define a function that counts the number of unique permutations contained within a string

> **Definition**: Given a string $s\in\Sigma_n^*$, let $P(s)$ represent the number of unique permutations covered by that string. 

Formally $P(s)$ is the number of items $p\in S_n$ such that we can find some index $i$ where $s_{i:i+n-1}=p$. Also note that $P(s)$ depends on $n$, however for brevity of notation we will represent as $P(s)$ instead of $P_n(s)$

*Note* that in the original 4chan post, Anon represented this quantity as $N_0$, however I think the different letters will help differentiate for teaching purposes.

> **Example**: If $n=3$, $P(12312)=3$

By **Lemma SE1**, we know that a permutation can only be covered if all characters are present, as such we know that
$$
P(s_{1:n-1})=0
$$
and thus by extension[^17]
$$
L(s_{1:n-1})-P(s_{1:n-1}) \geq n-1
$$

*Note* that the 4chan user calls $L(s)-P(s)=X_0(s)$, but I believe this additional notation to be unnecessary. 

We now consider what happens when we move from $s_{1:k}$ to $s_{1:k+1}$ with respect to $L$ and $P$. Since we are increasing our string by one character, $L$ must increase by $1$. However, $P$ may increase by $1$, but will not increase if we do not complete a permutation. As such we can perform some algebra to show that
$$
\begin{align}
L(s_{1:k+1}) - P(s_{1:k+1}) &\geq \left(L(s_{1:k})+1\right) - \left(P(s_{1:k})+1\right) \\\\
&\geq L(s_{1:k})-P(s_{1:k}).
\end{align}
$$
This means that as we increase the length of our string, the gap between $L$ and $P$ is non-decreasing, which means we can set
$$
\begin{align}
 L(s) - P(s) &\geq L(s_{1:n-1})-P(s_{1:n-1}) \\\\
 \implies L(s) - P(s) &\geq n-1 \\\\
 \implies L(s) &\geq P(s) + n -1.
\end{align}
$$

However, since $s$ is a superpermutation string, we know it must cover all possible permutations. Therefore
$$
    L(s) \geq n! + n - 1
$$
proving our **Lemma Bound B4**. Q.E.D.

---

### Step Two

In order to move up to the next stage, we need to understand the concept of "weighted edges" that the author comes up with. Specifically they envisioned a graph where each permutation is represented by a node, and nodes $u,v$ are connected via a directed edge if you can add additional characters to the end of the permutation represented by $u$ to get the permutation represented by $v$ *without passing through any additional permutations*. The "weight" or "cost" of this edge will be the number of characters required to be added. The author calls an edge with weight $k$ a $k$-edge.

For example, this would be a sequence of $1$-edges
$$
1234\xrightarrow{1} 2341\xrightarrow{1} 3412\xrightarrow{1} 4123
$$
because the string $1234123$ only adds one character at a time, but connects those permutations together. Comparatively, if we were to do $12342$, adding that $2$ to the end of $1234$ does not connect any permutations together, because $2342$ is not a permutation.

It is also important to note that $1234$ and $3412$ are **not** connected via a $2$-edge, even though it takes two characters to move between them. This is because $2341$ is intermediate between those two permutations, and thus they are instead connected via a path of two $1$-edges. A proper $2$-edge would look like instead 
$$
1234\xrightarrow{2} 3421
$$
because in $123421$ we take two characters to reach $3421$ and $2342$ is not a permutation. To put this into a precise definition

> **Definition**: Permutations $p_1, p_2\in S_n$ are connected via a $k$-edge from $p_1$ to $p_2$ if and only if you can create string $s$ by concatenating $k$ characters to $p_1$ such that $s_{1:n}=p_1$, $s_{k+1:n+k}=p_2$ but for all $1 \leq i < k$ then $s_{i+1:n+i}\not\in S_n$.

We have the following Lemmas

> **Lemma 4C1**: For every permutation $s\in S_n$ there is exactly one permutation $t\in S_n$ such that $s\xrightarrow{1} t$
{{% callout info %}}
<details>
<summary>Proof</summary>
Let $s$ be a $n$ character string that contains only a single permutation, and is thus $n$ characters long. We will then concatenate a character $c$ to the end $s$ so that $s'=s\cdot c$. In order to be a $1$-edge, this means that $s'_{2:n+1}$ must be a permutation, and since $s'_{1:n}$ is also a permutation the only valid option for $c$ that satisfies this is $s_1$.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

This proof shows us that the *only* way to take a $1$-edge out of a permutation is to move the first character of that permutation to the end. 

Now the reason that the previous Lemma worked intuitively is that the only character that can be tacked on to the end to create a new permutation is the first one, and that's because any other character would be too close to it other appearance and cause a repeat. In the same way, if we want to tack $2$ characters on to the back to create a $2$ edge we have to use the first $2$ characters in our permutation. This might make you think that we have two possible options for a $2$-edge out of any particular permutation, but that is actually not the case! Surprisingly

> **Lemma 4C2**: For every permutation $s\in S_n$ there is exactly one permutation $t\in S_n$ such that $s\xrightarrow{2} t$
{{% callout info %}}
<details>
<summary>Proof</summary>
Let $s$ be a $n$ character string that contains only a single permutation, and is thus $n$ characters long. We will then concatenate two characters $c_1c_2$ to the end $s$ so that $s'=s\cdot c_1\cdot c_2$. In order to be a $2$-edge, this means that $s'_{3:n+2}$ must be a permutation and $s'_{2:n+1}$ must not be a permutation. Similar to the previous argument, we know that $c_1,c_2$ must be the first two characters of $s$ however, $c_1$ cannot be $s_1$, otherwise that would be a $1$-edge and thus $s'_{2:n+1}$ would be a permutation. As such, we know that $c_2$ must be $s_1$ and $c_1$ must be $s_2$.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

Now with these two Lemmas, we can start considering how one might build a superpermutation string efficiently. Clearly, if it was possible we would always want to $1$-edges, as those increase our permutation count by $1$ while only using $1$ character. Unfortunately, we cannot take only $1$-edges as those actually form a cycle. We can see this from our previous example if we take one more $1$ edge
$$
1234\xrightarrow{1} 2341\xrightarrow{1} 3412\xrightarrow{1} 4123\xrightarrow{1} 1234
$$

> **Definition**: A cycle created by a sequence of $1$-edges is a $1$-cycle. 

> **Lemma 4C3**: Let $s\in S_n$. Taking $n$ $1$-edges will cover $n$ permutations (including $s$) before ending on the permutation covered by $s$.

This just states that the cycle contains $n$ items, and this can be proven many ways. My preferred thought is to use modular arithmetic, however I encourage the reader to think of their own specialty 🧑‍🍳.

Since taking a $1$-edge while inside of a $1$-cycle will keep you inside of it, if you want to jump to a different $1$-cycle then you **must** take a $k$-edge out when $k>1$. Note that this means that we are "wasting" a character, as it will be added to the string but will not cover a new permutation.

> **Definition**: Let $C_1$ be the set of all $1$-cycles, where $c\in C_1$ is a $n$-tuple of permutations covered by the represented cycle in order.

Note that since this is a cyclic structure, the choice of first element is not important, rather that the elements follow each other in sequence.

> **Lemma 4C4**: $C_1$ is a partition of the set of permutations of $n$ characters.
{{% callout info %}}
<details>
<summary>Proof</summary>
Given any permutation, we know via <b>4C1</b> that there is exactly one $1$-edge out. As such, we know that given any permutation we can use a $1$-edge to create a new one. Via <b>4C3</b> if we perform $n$ $1$-edges, we will make it back to our original permutation. As such, we are guaranteed that all permutations are contained within at least $1$ $1$-cycle.
</br>
However, since there is exactly $1$ $1$-edge from each permutation, it is not possible to have the same permutation be contained within two different $1$-cycles. If there was an overlap, the sequence of $1$-edges would be identical, and thus the same cycle. This means that all cycles are disjoint and cover the set of permutations. 
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

Since all $1$-cycles are $n$ permutations long, there could be future insights gleaned from the world of Algebra, considering this as as it relates to group theory and the symmetric group $S_n$, however that is for another day.

Also, we know that because each $1$-cycle contains $n$ permutations, there must be $(n-1)!$ distinct $1$-cycles.

We now have enough information to prove the next bound in our sequence

> **Lemma Bound B5**: Let $s\in\Sigma_n^*$ be a superpermuation with $n>2$ characters, then
$$
L(s) \geq n! + (n-1)! + n - 2
$$

In order to help us, we will define the following quantity

> **Definition**: Given $s\in\Sigma_n^*$, let $N(s)$ represent the number of distinct $1$-cycles that have had an element of theirs appear within permutations of $s$[^16].

Being more formal, we would say that $N(s)$ is the amount of elements $c\in C_1$ such that there exists an index $i$ such that 
$$
s_{i:i+n-1}\in c.
$$

Similar to the previous step, we will consider what happens now to the quantity $L(s)-P(s)-N(s)$. Since each permutation requires $n$ characters, we know that
$$
L(s_{1:n})-P(s_{1:n})-N(s_{1:n}) \geq n - 1 - 1 = n - 2
$$

Again, we consider what happens when we increase the substring that we look at from $s_{1:k}$ to $s_{1:k+1}$.  

1. We could have taken a $1$-edge to a new permutation. This would increase $L$ by $1$, increase $P$ by $1$, but not increase $N$. This is because a $1$-edge can not switch you to a new $1$-cycle via **4C3**.
2. We could have taken a $1$-edge to an already covered permutation. This would only increase $L$ by $1$.
3. We could have completed a $2$-edge to a permutation we have already covered. This would only increase $L$ by $1$.
4. We could have completed a $2$-edge to a permutation we have not covered in a $1$-cycle we have already seen before. This would increase $L$ by $1$, increase $P$ by $1$, but not increase $N$.
5. We could have completed a $2$-edge to a permutation in a new $1$-cycle. This would increase $L$ by $1$, $P$ by $1$, and $N$ by $1$. However, since we completed a $2$-edge, this means that the character $s_k$ would *not* have completed a new permutation and would have only increased $L$ by $1$. As such, completion of this $2$-edge will decrease by $1$, but that will just bring the total value to the same as when $s_{1:k-1}$ was observed. 

Summarizing this, we can see that 
1. If a $1$-edge is taken, then $$L(s_{1:k+1})-P(s_{1:k+1})-N(s_{1:k+1})\geq L(s_{1:k})-P(s_{1:k})-N(s_{1:k})$$
2. If a $2$-edge is taken, then $$L(s_{1:k+1})-P(s_{1:k+1})-N(s_{1:k+1})\geq L(s_{1:k-1})-P(s_{1:k-1})-N(s_{1:k-1})$$

We do not need to consider any higher order edges as of now, because they would for **B5** fall into a similar argument as point (5) above. 

Since all $1$-edges and $2$-edges will not decrease our total, we can state that for our final string $s$
$$
\begin{align}
L(s) - P(s) - N(s) &\geq L(s_{1:n})-P(s_{1:n})-N(s_{1:n}) \\\\
\implies L(s) - n! - (n-1)! &\geq n - 2 \\\\
\implies L(s) &\geq n! + (n-1)! + n - 2
\end{align}
$$
since at completion we will have seen every permutation, which completely covers every $1$ cycle. **Q.E.D.**

---

### Step Three

In this final step, we will prove the desired bound that to this day is still the best lower bound that we have. In this step we take a further look at what happens when we take a $2$-edge out of a specific $1$-cycle. Watch what happens in the following example when $n=5$
$$
\begin{align}
&12345\xrightarrow{1}23451\xrightarrow{1}34512\xrightarrow{1}45123\xrightarrow{1}51234 \\\\
\xrightarrow{2}&23415\xrightarrow{1}34152\xrightarrow{1}41523\xrightarrow{1}15234\xrightarrow{1}52341\\\\
\xrightarrow{2}&34125\xrightarrow{1}41253\xrightarrow{1}12534\xrightarrow{1}25341\xrightarrow{1}53412\\\\
\xrightarrow{2}&41235\xrightarrow{1}12354\xrightarrow{1}23541\xrightarrow{1}35412\xrightarrow{1}54123\\\\
\xrightarrow{2}&12345
\end{align}
$$
after traversing $4$ $1$-cycles, we end up back in our original permutation! By taking repeated $2$-edges, we have created this cycle of cycles. Btw, in case you were curious, the full string for this cycle would look like the following
$$
s=12345123415234125341235412345
$$
which uses $29$ characters ($27$ if you exclude the last $4,5$ which covers the existing first permutation) to cover $20$ permutations. We will provide the following definition

> **Definition**: The cycle of $1$-cycles formed by taking successive $2$-edges after taking $n-1$ $1$-edges in a $1$-cycle is called a $2$-cycle, with set $C_2$ being the set of all $2$-cycles with $c\in C_2$ being the $n\cdot (n-1)$-tuple of permutations with the first element being one of the entrypoints of $c$.

Soon we will discuss what we mean by "entrypoints", however let us first show why a $2$-cycle contains $n\cdot (n-1)$ elements.

> **Lemma 4C5**: Let $n$ be the number of characters of our permutations, let $c_1, c_2, c_3,\ldots, c_n\in C_1$ be the sequence of $1$-cycles formed by starting with some $p\in c_1$, taking each $1$-cycle to all permutations via $n-1$ $1$-edges, and then moving to a new $1$-cycle via a $2$-edge. Then $c_1=c_n$.

This Lemma is saying two things: first that $2$-edges actually create cycles, and that those cycles contain $n-1$ $1$-cycles within them.
{{% callout info %}}
<details>
<summary>Proof</summary>
The following argument works for any initial permutation, so without loss of generality, lets assume that our initial permutation of $n$ characters is 
$$
12345\ldots (n-1)n.
$$
After completing $n-1$ $1$-edges, we have shifted the first character to the end $n-1$ times which means our resulting final permutation will begin with what the final character was at the beginning of the $1$-cycle. In this case that would be here
$$
n1234\ldots (n-1).
$$
The $2$-edge of the permutation takes the first character of the string and moves it to last, the second character and moves it to second last, and shifts all other characters left by $2$. Since we are taking the two edge at the end of the $1$-cycle, the first character will be whatever was last in the initial permutation, in this case $n$, and the second character will be whatever was first in the initial permutation, in this case $1$. 
</br>
However, notice that we have now entered into a new $1$-cycle after taking the $2$-edge with $n$ as the last character. This means that after completion of this $1$-cycle and taking a $2$-edge to the next one, $n$ will again be the final character in the first permutation of that following $1$-cycle. As such, we can say that the first permutation of every $1$-cycle within our larger $2$-cycle has $n$ as its last character.
</br>
Since the first permutation of each $1$-cycle will have as the second-to-last character the first character of the previous $1$-cycle, with all others shifted left, we can see that this actually forms a cycle for the other $n-1$ characters. We can visualize this easier by observing when $n=5$ the sequence of first permutations of the $1$-cycles in our $2$-cycle.
$$
12345\rightarrow 23415\rightarrow 34125\rightarrow 41235\rightarrow 12345.
$$
We can see that $5$ remains invariant, whereas the other characters form a cyclic rotation after each $1$-cycle. In order to return to the first permutation, we need to ask how many steps would be required in order to cycle through $n-1$ characters, which is $n-1$. From this we know that eventually we will cycle back to the initial permutation after completing $n-1$ $1$-cycles.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

A potential future direction of inquiry would be to observe that those first permutations of their respective $1$-cycles form a $1$-cycle itself with the first $n-1$ characters. Perhaps some insights could be gleaned from this recursive structure, but that's for another day.

Now we need to discuss the big difficulty when it comes to $2$-cycles, they are actually not distinct from each other. From the above proof of **4C5**, we can see that the first permutation of each $1$-cycle will have the same final character, however all the other characters will be shifted. This means that the $1$-cycle that you enter in after the $2$-edge depends on whatever the permutation you started the $1$-cycle on was[^18], and thus every $1$-cycle is part of $n$ different $2$-cycles! In total there are $n\cdot (n-2)!$ different $2$-cycles that overlap with each other. From this we will define the following two terms

> **Definition**: Let $s$ be a string that contains only $1$-cycle $c\in C_1$, then the **entrypoint** of $c$ is the first covered permutation by $s_{1:n}$. 

> **Definition**: Let $E_2$ be the set of all $2$-cycles entrypoints, where for each $c\in C_2$ we have $e=(p_1,p_2,\ldots,p_{n-1})\in E_2$ with $p_1,p_2,\ldots, p_{n-1}\in S_n$ are the *entrypoints* for the $1$-cycles present within the $c$ in the order that they appear within $c$.

> **Lemma 4C6**: $E_2$ forms a partition of the set $S_n$
{{% callout info %}}
<details>
<summary>Proof</summary>
For all $p\in S_n$, it is trivial to show that there must exist some $e\in E_2$ such that $p\in e$. Simply start with $p$ and perform the process described in <b>4C5</b> to create a $2$-cycle. This then will correspond to an element of $C_2$.
</br>
In order to show uniqueness, assume that there are some values $e_1, e_2\in E_2$ with corresponding elements $c_1, c_2\in C_2$ such that $e_1\neq e_2$ but there is some $p_1\in e_1\cap e_2$. Since $e_1,e_2$ both contain $p_1$ as an entrypoint, we may start both of them using $p_1$. Since $1$-cycles form a partition per <b>4C4</b> we can know that both $c_1,c_2$ must share the same initial $1$-cycle and will complete this $1$-cycle on the same permutation. However, taking a $2$-edge to the next entrypoint will create the same permutation $p_2$ for both $c_1,c_2$ meaning that $p_2\in e_1, e_2$. Repeating the same process as before until we complete the $2$-cycle shows that we contradict the initial claim that $e_1\neq e_2. 
<b>Q.E.D.</b>
</details>
{{% /callout %}}

This is nice because we now can see that while each individual permutation $p\in S_n$ is a part of $n$ $2$-cycle processes, each permutation can be the entrypoint of exactly $1$ $2$-cycle. In the original 4Chan proof, the anonymous user does not consider this partition $E_2$ and only works with $C_2$, we hope that our additional definition will make things easier to work with.

As stated prior, $|E_2|=|C_2|=n\cdot (n-2)!$ which is straightforward via **4C6** since each element $c\in E_2$ contains $n-1$ items and there is a correspondence between $E_2,C_2$. 

Still, this makes things more challenging, and as such we will need to observe more cases to prove our final theorem

> **Theorem 4Chan Bound B6**: Let $s\in\Sigma_n^*$ be a superpermutation string, then
$$
L(s) \geq n! + (n-1)! + (n-2)! + n - 3
$$

Similar to prior, we will define a counting quantity that will help us get to the desired bound

> **Definition**: Let $T(s)$ be the number of distinct $2$-cycles that we have entered into. 

Note that when we say $2$-cycles that we have entered into, we can only increase $T(s)$ if we observe a new **entrypoint** of a $1$-cycle that is not already found. In order to formalize it, we will say that $T(s)$ is equal to the number of elements $d\in E_2$ with the following property. There exists a $p\in d$, where $p\in c$ with $c\in C_1$, such that we can find some substring $p=s_{i:i+n-1}$ but $s_{i-1:i+n-2}\not\in c$. In words, mark some permutation as "seen" inside its $C_2$ element if we got to it via taking something other than a $1$-edge, thus making it an entrypoint. The beauty of this is that we cannot increase $T(s)$ more than one time for each $2$-cycle. This also provides us an analog to $N$ in **Step 2** where we said we were counting "observed" $1$-cycles. 

> **Lemma 4C7**: Taking a $1$-edge cannot increase the value of $T(s)$

This follows directly from the definition, and the fact that a $1$-edge does not change your $1$-cycle. 

We will observe similar to the previous two cases, the quantity which for notational brevity we will denote with $X_2(s)$ as done in the original 4Chan post
$$
\begin{align}
X_2(s_{1:n}) &= L(s_{1:n})-P(s_{1:n})-N(s_{1:n})-T(s_{1:n}) \\\\
&\geq n-3
\end{align}
$$
and now we must show that our sum here increases overall. Let us consider what happens when we increase our characters from some substring $s_{1:k}$. $T(s)$ can only increase when we have entered into a new $2$-cycle, which means that in the event that we have not done so, then we can refer to the proof of **Step 2** to show that our $X_2$ must not decrease between $s_{1:k}\rightarrow s_{1:k+1}$ if taking a $1$-edge or $s_{1:k-1}\rightarrow s_{1:k+1}$ if taking a $2$-edge.

We can now limit our cases to when we are increasing the value of $T(s)$. The scenarios in which we can increase $T$ are
1. We take a $3$-edge or higher to a new $2$-cycle. In this case we see a new permutation, $1$-cycle, and $2$-cycle, however we increase $L$ by at least $3$. This means that $L(s_{k-2})-P(s_{k-2})-N(s_{k-2})-T(s_{k-2})\leq L(s_{k+1})-P(s_{k+1})-N(s_{k+1})-T(s_{k+1})$ is non-decreasing in the $3$-edge case[^19]. 
2. We take a $2$-edge from a permutation from our current $1$-cycle $c\in C_1$ that we have already seen and looped back around to repeat. This $2$-edge will increase $P,N,T$ by $1$, and $L$ by only $2$, however since we are moving from a permutation that we had previously already seen, that would not have increased $P,N,T$ but would have increased $L$. Thus we can say  $L(s_{k-2})-P(s_{k-2})-N(s_{k-2})-T(s_{k-2})\leq L(s_{k+1})-P(s_{k+1})-N(s_{k+1})-T(s_{k+1})$

However, now we find a snag and the most difficult part. What happens if we exit the current $2$-cycle we are in early by taking a $2$-edge out to a new $2$-cycle? In this case we increase $P,N,T$ by $1$ but $L$ by only $2$! This screws up our whole idea of non-decreasing! This is a direct consequence of us adjusting the 4Chan definition of $N$ to make it slightly less ambiguous.

The way we will get around this is to show that if we "start" at $s_{1:n}$ then when we complete our process at $s$, then we will not have dipped down below that initial amount, by showing all those steps that decrease our sum must have some corresponding step that strictly increases it.

Suppose we are in some $2$-cycle and we exit it before completing it, which means that at some other location in $s$ we must have already covered the remaining permutations of this $2$-cycle. If we wanted to leave this $2$-cycle at the end of one of its corresponding $1$-cycles, then we would be unable to do so via a $2$-edge as it would keep us in the same $2$-cycle, which places us into point (1) from above, and as such we only need to consider what happens to the "leftover" permutations of a $1$-cycle that we exit in the middle of[^20].

It is clear that these leftover permutations that we leave behind from exiting our $1$-cycle early must be consecutive within the $1$-cycle. If we were to cover these permutations via the same $2$-cycle, then we are done, as we would need to cover at least one permutation that we have already covered in order to get to the leftovers, which will increase $L$ by $1$ but not $P,N,T$, which would sufficiently offset the previous deficit. The same would be the case for any $2$-cycle that has an entrypoint within the $1$-cycle of the leftovers that has already been covered. We can now limit all our remaining scenarios to $2$-cycles who's entrypoints have not been covered yet.

If we have already seen the $2$-cycle which has a leftover permutation as an entrypoint, then taking at least a $2$-edge into the $1$-cycle with our leftover permutation will increase $L$ by at least $2$, but only $P$, which sufficiently offsets the previous deficit. This finally means that we now only need to consider when we are entering a new $2$-cycle into one of our leftover permutations as the initial entrypoint. In this case both $P,T$ increase, so if we take a higher order edge than $2$ into this $2$-cycle then we are done. 

If we enter into our $2$-cycle with initial entrypoint being our leftover permutation via a $2$-edge, then we could have case (2) from the list above, however $N$ would not be increased, which offsets our deficit. Our only remaining option is to exit some other $1$-cycle midway through via $2$-edge, which would then keep our sum neutral and *not* offset the deficit. However, notice what we just did here: we actually just created more leftover permutations for ourselves! In the same vein, depending on our entrypoint, we may have "split" our original leftover permutations such that you would need to traverse the entire $1$-cycle to cover them all; in this scenario you either need to travel through already covered permutations, or exit the $2$-cycle which creates more "leftovers", which we will explain below. Note that throughout all of these situations the deficit by these leftovers associated with the original cycle we did not complete has not raised greater than $1$.

Imagine that we continually repeat this process, creating new leftovers to cover the old ones. Eventually we must cover the final set of leftovers, however this cannot be done via a $2$-edge in the middle of a $1$-cycle (as that would create new leftovers and these are the last ones) which means that the only other options we have to deal with them are options that offset the deficit.

With this, we can finally say that the quantity of our final string
$$
L(s)-P(s)-N(s)-T(s)\geq L(s_{1:n})-P(s_{1:n})-N(s_{1:n})-T(s_{1:n}) \geq n-3.
$$
Since each $2$-cycle covers at most $n\cdot (n-1)$ permutations, then we must see **at least** $\frac{n!}{n\cdot (n-1)}=(n-2)!$ distinct $2$-cycles occur, thus
$$
\begin{align}
L(s)-P(s)-N(s)-T(s) \geq L(s)-n!-(n-1)!-(n-2)! &\geq n-3 \\\\
\implies L(s) &\geq n!+(n-1)!+(n-2)!+n-3
\end{align}
$$

**Q.E.D.**

Holy fuck that was brutal.

---

The proof we will perform here is a large effort of case work, which I will try to make as straightforward to follow as possible. Lets consider the set of cases that can occur when we start at some substring $s_{1:i}$ of superpermutation string $s$ and proceed to increase the character count by $k$ to represent the completion of some $k$-edge. We consider the completion of the entire edge as a single "step" to make the discussion less annoying to deal with the many indices and edge cases. Remember that per definition of a $k$-edge, for the first $k-1$ characters in said edge, no permutations can be covered, meaning in those indices only $L$ can be increased. Values $P,N,T$ can *only* increase by at most $1$ upon completion of a $k$-edge.

1. $T(s_{1:i+1})=T(s_{1:i})$: Case $1$ occurs when we increase our character count, but do not enter into a new $2$-cycle, instead staying inside the same one or entering one we've already seen before. In this case $X_2=L-P-N$, which is the same quantity as in **Step 2**. From **Step 2** we know that upon completion of any $k$-edge the quantity $L-P-N$ is non-decreasing, we know that in this case $X_2$ is non-decreasing as well.
2. $T(s_{1:i+1})=T(s_{1:i})+1$: Case $2$ occurs when we increase our character count *and* enter into a new $2$-cycle.

There are multiple scenarios that can occur during Case $2$, so let us observe them. When referring to this next list, we will denote it as $2.x$ to represent that it is list item $x$ of the original Case $2$, which will hopefully make these cases easier to track.

1. The completed $k$-edge is $k\geq 3$. Case $2.1$ sees that we increase $L$ by at least $3$, while increasing $P,N,T$ each by at most $1$, since its possible to be in a new $2$-cycle but not a new $1$-cycle or permutation. As such in this case $X_2$ is non-decreasing.
2. The completed $k$-edge is $k=2$: Case $2.2$ cannot occur $n-1$ edges from the entrypoint of the current $1$-cycle of the current $2$-cycle. This is because that would jump us to the next entrypoint in the $2$-cycle which would not increase $T$. As such we must take this $2$-edge out from some other permutation $p$ in the current $1$-cycle.

We will now split Case $2.2$ into the following two categories

1. We take a $2$-edge out of our current $1$-cycle after having taken more than $n-1$ $1$-edges, from a permutation that is not $n-1$ $1$-edges away from the entrypoint. Case $2.2.1$ represents what happens if we complete our current $1$-cycle, take more $1$-edges to cycle back to the front, and then take a $2$-edge out to a new $2$-cycle. In this case, we take a $2$-edge, which increases $L$ by $2$, to a new $2$-cycle which increases $T$ by $1$, and $P,N$ each by at most $1$. However, since we are exiting from a permutation we have already covered, the step prior to taking the $2$-edge increases $L$ by $1$ only. Thus $X_2(s_{1:i-1})\leq X_2(s_{1:i+2})$ and $X_2$ over this interval cannot decrease further than $X_2(s_{1:i-1})$
2. We take a $2$-edge out of our current $1$-cycle after having taken less than $n-1$ $1$-edges. This means that the current $1$-cycle that we are in will be exited early.

Before continuing, let us define the following terminology

> **Definition**: Given s\in\Sigma_n^* and some $1$-cycle $c\in C_1$ such that there exists a $p\in c$ that is covered by $s$. We say some permutation $q\in c$ is a **leftover** of $c$ if it is not covered by $s$.

Simply put, leftovers are permutations of one cycles that have only been partially covered. This means that at some point we will need to return back to that $1$-cycle if we want to build a superpermutation string.

Now let us split case $2.2.2$ into $2$ cases

1. At least one permutation that was covered before exiting the one cycle had already been covered before. Suppose this permutation occurred at index $j$, then by the same argument as $2.2.1$ we can say that $X_2(s_{1:j})\leq X_2(s_{1:i+2})$ after taking the $2$-edge.
2. All permutations visited before exiting this one cycle were not previously covered.

We will now split $2.2.2.2$ into $2$ cases. Just to provide a refresher on where we are, we are analyzing the scenario in which we have found we entered into a new $2$-cycle to increase $T$ after taking a $2$-edge after taking less than $n-1$ $1$-edges in our current cycle, and all the permutations visited in this $1$-cycle were new and thus each increased $P$.

1. The $1$-cycle that we are entering is one we have already visited before. This means that we increase $L$ by $2$, $T$ by $1$, and possibly $P$ by $1$. Note this is possible because we may have visited this $1$-cycle before, but via a different entrypoint. In this case $X_2$ is non-decreasing.
2. The $1$-cycle that we are entering into has not been visited before. We have now hit a snag, as $L$ increases by $2$, but all of $P,N,T$ are guaranteed to increase, meaning that this scenario does decrease. 

From all of our cases, we can now see that exactly $1$ is a villain scenario: $2.2.2.2.2$. Note that the original 4chan proof gets around this by having a different definition of $N$ doesn't count non-completed cycles you're not currently in, my definition I feel is slightly more rigorous but is looking more and more like a decision I regret as I continue writing. To simplify our writing I will define the following

> **Definition**: Let $s\in\Sigma_n^*$. Let $D(s)$ be the **debt counter** which counts the number of times Case $2.2.2.2.2$ occurs, and let $R(s)$ be the **repayment counter** that counts the number of steps that strictly increase $X_2$.

Since our eventual end goal is to show that any superpermutation string $s$ has 
$$
X_2(s) \geq X_2(s_{1:n})
$$
then it is now sufficient to show that $R(s)\geq D(s)$ to prove the claim. The way we will do this is by showing that for every piece of "debt" we incur by not completing a $1$-cycle, we will eventually *have* to pay it back eventually when we reenter into our $1$-cycle that we exited previously. Since we are now showing how we defeat the "villain" scenarios, for notational brevity we will represent case $2.2.2.2.2$ as Case $V$. We split Case $V$ into the following.

Let $c\in C_1$ be a cycle that was exited early and thus incurring a point of "debt". Since it was exited early, there must be some leftovers in $c$, and thus we must return to $c$ at some point later in $s$.

1. We re-enter $c$ via a $k$-edge with $k\geq 3$. This will increase $L$ by $k$, and $P,T$ by at most $1$ each. Since this $1$-cycle had already been visited previously, then $N$ does not increase, and thus in this step we increase $R$ by $1$, paying off the debt.
2. We re-enter $c$ via a $2$-edge without entering a new $2$-cycle. This scenario accounts for the situation in which we were already within a $2$-cycle or entered into one we had already seen before. In this case $L$ increases by $2$, $P$ can increase by $1$, but $N,T$ cannot increase, and thus we can increase $R$ and pay our debt.
3. We re-enter $c$ via a $2$-edge while entering a new $2$-cycle. Notice that this can actually only via Case $2.2$. If we have that the scenario we find ourselves is $2.2.1$ and we have cycled over this $1$-cycle more than once, then we pay off our debt easily. The additional considerations now come from what happens if we enter into $c$ from exiting some *other* cycle early, and thus creating potentially more leftovers!

Let us split Case $V.3$ to further analyze.

1. We re-enter $c$ into a permutation $p$ that we have already previously covered. In this case we increase $L$ by $2$, $T$ by $1$, but $P,N$ do not increase. This increases $R$ and pays back the debt.
2. We re-enter $c$ into a permutation $p$ that we have not already covered. In this scenario $L$ increases by $2$, $P, T$ by $1$. This does *not* increase the debt but it does not repay it.

Case $V.3.2$ is interesting when we think of what arrived us into this situation. As mentioned in the description of Case $V.3$, by exiting another $1$-cycle early in order to re-enter $c$, we may have created new leftovers. Observe that even in the event that we create new leftovers, in neither scenario $V.3.1$ or $V.3.2$ are we actually increasing $D$. Rather we in fact pay it off in scenario $V.3.1$, or we keep it the same in $V.3.2$. Imagine that the cycle we are exiting in $c_2\in C_1$, for scenario $V.3.2$ we will say that we are "transferring" the debt to $c_2$ in order to track how it gets repaid. 

Of course, note that debt does not actually connect to a particular cycle, but in order to show how $c$ is guaranteed to get repaid, we will follow what happens with the leftovers of $c_2$ that were created in order to cover the leftovers of $c$. 

Also note, that it is possible for us to not cover all the leftovers of $c$ when we reach that $1$-cycle for the second time, and by extension create new debt. In this scenario we will treat these as new leftovers that will be repaid on their own later via the arguments we have discussed.

Now consider the leftovers of $c_2$. For these it is possible for Case $V.1,V.2,V.3.1$ to all occur which would thus pay off the debt of the original leftovers $c$ by increasing $R$, finishing this chain. However, if Case $V.3.2$ occurs again, then we will create another set of leftovers $c_3$ that will have the debt of $c_2$, and by extension $c$, transferred to it. We then create a sequence of leftovers $c, c_2, c_3, c_4, \ldots$ that all are connected to the same debt. 

**Importantly**, since $s$ is finite, this sequence must at some point terminate, meaning that we covered $c_m$ without creating any new leftovers. Since we know that we cannot cover $c_m$ without creating new leftovers than we can cover it via scenarios $V.1, V.2$ directly. However, what would happen if we were to enter $c_m$ via a $2$-edge exiting some $1$-cycle early?

Well, since we cannot create any new leftovers, this means the remaining permutations in the $1$-cycle we are exiting must have already been visited. This means that we must have already seen this $1$-cycle at some point, and thus would not have increased $N$ when entering it initially, which is enough to ensure that we have paid off the debt associated with $c$.

We can now be confident that since the initial debt is $0$ at $X_2(s_{1:n})$, then this must be less than or equal to $X_2(s)$.

To complete, we know that $P(s)=n!$, $N(s)=(n-1)!$. For $T(s)$, observe that a $2$-cycle contains $n\cdot (n-1)$ permutations. That means in order to cover all $n!$ permutations, we must have entered *at least* $(n-2)!$ distinct $2$-cycles. So $T(s)\geq (n-2)!$

With this we can say
$$
\begin{align}
X_2(s) \geq L(s) - n! - (n-1)! - (n-2)! &\geq n - 3 \\\\
\implies L(s) \geq n!+(n-1)!+(n-2)!+n-3.
\end{align}
$$

**Q.E.D.**

With this, we now know that the bounds of the Haruhi problem are 
$$
93884313611\leq L(s) \leq 93924230411
$$
meaning our solution is somewhere within this gap of $11!$[^21], which is approximately $40$ million!






[^1]: https://docs.python.org/3/reference/datamodel.html#sequences
[^2]: https://www.gregegan.net/SCIENCE/Superpermutations/Superpermutations.html
[^3]: Natural numbers start at $0$ you heathens
[^4]: https://en.wikipedia.org/wiki/Haruhi_Suzumiya#Anime
[^5]: https://oeis.org/A180632
[^6]: https://web.archive.org/web/20181024185958/http://4watch.org/superstring/
[^7]: https://njohnston.ca/2013/04/the-minimal-superpermutation-problem/
[^8]: https://arxiv.org/abs/1408.5108
[^9]: https://njohnston.ca/2014/08/obvious-does-not-imply-true-the-minimal-superpermutation-conjecture-is-false/
[^10]: I have been unable to find the archive of the second thread. I also have been unable to find any later discussions related to the problem after this thread
[^11]: https://web.archive.org/web/20181024190314/https://warosu.org/sci/thread/3751105
[^12]: https://math.stackexchange.com/questions/15510/what-is-the-shortest-string-that-contains-all-permutations-of-an-alphabet
[^13]: https://mathsci.fandom.com/wiki/The_Haruhi_Problem
[^14]: Engen, M., & Vatter, V. (2021). Containing All Permutations. The American Mathematical Monthly, 128(1), 4–24. https://doi.org/10.1080/00029890.2021.1835384
[^15]: I will personally get on my soapbox that this might get to me due to my own background as a hobbyist in mathematics, as well as my persistent observation that mathematicians are incredibly quick to discard the ideas of anyone who's not in the tight problem space community without its customs and language. 
[^16]: This is a slightly different formulation than the original proof, however we do so to account for the scenario in which we are consistently starting new $1$-cycles without completing them.
[^17]: We have this as greater than or equal to to show that its possible the first $n$ characters may not contain a permutation at all.
[^18]: You could also say that it depends on the final permutation prior to the $2$-edge, whichever representation you prefer.
[^19]: Adjust as needed if the edge is larger than $3$.
[^20]: In the event that, for example, you perform $3$ $1$-cycles in some particular $2$-cycle, you would need to take a higher order edge out, and then cover that $1$-cycle at some other point, in some other $2$-cycle. So limiting ourselves to just these straggler permutations is not a problem.
[^21]: Apparently Robert Houston[^14] has shown that this bound can be improved by $1$, but I have not seen that anywhere except an off comment within Engen and Vatter's paper.