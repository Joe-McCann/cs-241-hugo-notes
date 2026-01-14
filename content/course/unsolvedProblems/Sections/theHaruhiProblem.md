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
is the *factorial* function. 

If we have objects $1,2$ and we wanted to write down all possible permutations, we would see that there are $2!=2$ of them
$$
12,21.
$$

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

It is a natural question to ask what the shortest string would be that covers all possible permutations for $n$ characters. Specifically, this is called the **superpermutations** problem. 

In our previous example of $12221$ we can see this string is garbage and sucks because there is a ton of unnecessary $2$s. In fact, we can simply concatenate all permutations together to improve our sequence to only $4$ characters
$$
s=1221.
$$

But even this sucks, because there is no rule that says our permutations cannot overlap! As such, the following string is actually optimal
$$
s = 121.
$$
As we can see, the permutation $12$ is covered by $s_{1:2}$ and $21$ is covered by $s_{2:3}$. Of course, this is not the only optimal string, as we can create another one via relabeling $212$. We can see that this string has a length of $3$ characters, which we will measure with the following function

> **Definition**: Let $L:S\rightarrow \mathbb{N}[^3]$ be the function that takes a string as an input and returns the length of said string

So $L(123)=3$ and $L(5437548)=7$

I'm now going to commit a cardinal sin here by introducing my own notation, but I think it will be helpful later so 🤷

> **Definition**: Let $\sigma_n$ be the set of all minimal strings of $n$ characters that cover all permutations. 

More formally we would say that if $s$ is a string that covers all permutations, and $s'\in\sigma_n$, then 
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

Here we will include some basic lemmas and theorems that will we use later on in the page. Perhaps a real mathematician, or someone with half of a functioning brain, might find these so obvious they're not even worth writing down, much less proving. Sadly, I am neither a real mathematician, no a person with more than $2$ functioning brain cells, and thus I find it quite helpful to formally write them to reference later. 

I will label these with **SE** to refer to "stupid easy" for when referenced in the future.

The first two are related to what actually constitutes a permutation in our string, and when we can expect to find one. For more casual readers who the language of the proof might get a bit technical, just know that we are proving
1. You can't cover a permutation with some sequence if you are missing a character or contain a duplicate
2. Any sequence of $n$ characters cannot more than one permutation at a time

> **Lemma SE1**: A string $s$ that contains $n$ characters covers a permutation if and only if all $n$ characters are present within the string exactly once. 

{{% callout info %}}
<details>
<summary>Proof</summary>
For simplicity sake, we will define that the characters of our string are the digits $1,2,\ldots, n$ which is the set $\mathbb{Z}/n$. We first being by stating that a permutation $p$ is a bijection 
$$
p:\mathbb{Z}/n\rightarrow \mathbb{Z}/n
$$
where we can think of this mapping our indexes to the items in those indexes. If it is a permutation, then it must shuffle everything around, but still include everything, hence being a bijection. For the remainder of the proof, we will consider any given string as a function $s$
$$
s:\mathbb{Z}/n\rightarrow \mathbb{Z}/n
$$
</br>
For the forward direction, consider that we know that $s$ covers a permutation. This means by definition that $s$ is bijective and thus is both injective (every character appears only once) and surjective (no characters are missing). 
</br>
For the reverse direction, if $s$ contains every character exactly once, this means that it is both injective and surjective, and by extension is a permutation
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
Assume that our string $s$ covers at least two distinct permutations $p_1$ and $p_2$ where $p_1\neq p_2$. However, since $s$ covers the permutations we know that
$$
\begin{align}
p_1(1)=&s(1)=p_2(1) \\
p_1(2)=&s(2)=p_2(2) \\
&\vdots \\
p_1(n)=&s(n)=p_2(n)
\end{align}
$$
which means that $p_1=p_2$ contradicting our claim that they are two distinct permutations.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

We now have enough to create our first simple bounds

> **Trivial Bounds B1**: Let $s\in\sigma_n$, then $n! \leq L(n) \leq n\cdot n!$
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

> **Theorem Recursive Bound B2**: Let $s$ be an $n$ character super permutation string generated by the recursive algorithm, then 
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

On August 21st, 2014 Robin Houston published a document to arXiv providing a counterexample for **C1**[^8] when $n=6$ and proving it false for $n\geq 6$. However, according to Michael Engen and Vincent Vatter in their 2019 article **Containing All Permuations**[^14] it was not truly recognized in the community as serious

> *Michael Engen and Vincent Vatter*: At least since a 2013 blog post of Johnston, it had been known that there was an argument (on a website devoted to anime) claiming to improve on the lower bound ... However, the argument was far from what most mathematicians would consider a proof, and there had been no efforts to make it into one, in part because the claimed lower bound was so far from what was thought to be the correct answer at the time. However, Egan’s \[2018\] breakthrough quickly inspired several participants of the Superpermutators group to re-examine the argument.

The breakthrough of Egan at described in Engen and Vatter's was to create an algorithm that was able to create superpermutation strings of the following length

> **Theorem Egan Bound B3**: Let $s$ be a superpermutation string of $n$ characters generated by Egan's Algorithm[^2]. Then
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

## The 4chan Proof

I will now attempt to explain the 4chan proof of the lower bound here, sticking as close to the original argument and terminology as possible. In the paper by Engen and Vatter they "formalize" the proof by modifying the language to fit that of the literature.

However, I personally am slightly irritated by this as the argument of the 4chan proof was not taken seriously for several years by this group due who then had the interesting decision to throw shade on it *"the argument was far from what most mathematicians would consider a proof"*, all before providing an argument that feels less rigorous than the effort performed by the 4chan poster[^15].

As such, I wish to attempt to clean up the 4chan argument and present it in a pretty way so that it can be observed in the way the original anonymous user themselves thought of it. Throughout the proof effort, I will hypothesize about what may have potentially led the user to these conclusions for the sake of teaching.

### Step One

The proof follows a straightforward structure, we will imagine that we have some superpermutation string $s$ and then look at increasing sequences of characters. As we increase and add more characters to the subsequence that we are looking at, we will be keeping track of some counters, and will then show that those counters must always be smaller than the length of $s$. This will help us bound the string size.

We will start with the simplest bound, which is just a slightly improved version of **B1**. Steps one and two will be used almost as stepping stones to get us to the eventual desired bound, so I will not label them. 

> **Lemma**: Let $s$ be a superpermuation string of $n$ characters, then
$$
L(s) \geq n! + n - 1
$$

We will first define a function that counts the number of unique permutations contained within a string

> **Definition**: Given a string $s$, let $P(s)$ represent the number of unique permutations covered by that string

*Note* that in the original 4chan post, Anon represented this quantity as $N_0$, however I think the different letters will help differentiate for teaching purposes.

> **Example**: If $n=3$ $P(12312)=3$

By **Lemma SE1**, we know that a permutation can only be covered if all characters are present, as such we know that
$$
P(s_{1:n-1})=0
$$
and thus by extension
$$
L(s_{1:n-1})-P(s_{1:n-1}) = n-1
$$

*Note* that the 4chan user calls $L(s)-P(s)=X_0(s)$, but I believe this additional notation to be unnecessary. 

We now consider what happens when we move from $s_{1:k}$ to $s_{1:k+1}$ with respect to $L$ and $P$. Since we are increasing our string by one character, $L$ must increase by $1$. However, $P$ may increase by $1$, but will not increase if we do not complete a permutation. As such we can perform some algebra to show that
$$
L(s_{1:k+1}) - P(s_{1:k+1}) \geq \left(L(s_{1:k})+1\right) - \left(P(s_{1:k})+1\right) = L(s_{1:k})-P(s_{1:k}).
$$
This means that as we increase the length of our string, the gap between $L$ and $P$ is non-decreasing, which means we can set
$$
\begin{align}
 L(s) - P(s) &\geq L(s_{1:n-1})-P(s_{1:n-1}) \\\\
 L(s) - P(s) &\geq n-1 \\\\
 L(s) &\geq P(s) + n -1.
\end{align}
$$

However, since $s$ is a superpermutation string, we know it must cover all possible permutations. Therefore
$$
    L(s) \geq n! + n - 1
$$
proving our Lemma. Q.E.D.

### Step Two




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
