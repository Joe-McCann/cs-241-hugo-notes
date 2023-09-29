---
title: Pigeonhole Principle
linktitle: Pigeonhole Principle
toc: true
type: book
date: 2023-05-24
draft: false
tags:
    - cs241
    - functions
    - pigeonhole

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 245
---

## The Hardest Math Theorem ☠️

The pigeonhole principle is probably the theorem in math that scares me more than anything else in the world, because it is so powerful and fundemental, but actually applying it can be some of the most challenging problems a math student can face. This is particularly shocking because the theorem is so crazy easy, that you would almost think there is no point in having it.

I feel that theres likely just a name attached to it so mathematicians don't forget it but who knows.

> **Theorem Pigeonhole Principle**: Suppose you want to place $m$ pigeons into $n$ holes. If you have more pigeons than holes, $m>n$, then at least one hole will have more than one pigeon.

{{% callout info %}}
<details>
<summary>Proof</summary>
Let $P$ be the set of pigeons and $H$ be the set of holes. If $f:P\rightarrow H$ is a function that maps a pigeon to a hole, and $|P|>|H|$ (more pigeons than holes), then $f$ cannot be injective. Since $f$ is not injective there must exist at least one pair $x,y$ such that $x\neq y$ and $f(x)=f(y)$; this means there are at least two pigeons in the same hole.
</br>
QED
</details>
{{% /callout %}}

Why are people putting pigeons into holes? I have no idea, but hey its funny and mathematicians are goofballs.

If we want to make it super hard, we can make the theorem more general, if we were to divide all the pigeons evenly between the holes, we can also say

> **Theorem General Pigeonhole Principle**: Suppose you want to place $m$ pigeons into $n$ holes. At least one hole must have $\lfloor \frac{m}{n} + 1\rfloor$ pigeons.

For example, if I have $10$ pigeons and $4$ holes, then at least one hole must have $3$ pigeons. So fucking hard huh?

### Example Problems

You might be thinking *"Joe, how could this shit be hard?"* Well here are some example problems that all use the pigeonhole principle. Good luck lmfao.

> **Example**: The oldest person in the world is about $117$ years old, and there are around $8$ billion. Prove that at least two people on Earth were born during the same second.

{{% callout info %}}
<details>
<summary>Proof</summary>
Since everyone on the Earth is younger than $118$ years old, we can see that there are less than $$118\cdot 366\cdot 24\cdot 60\cdot 60 = 3731443200$$ possible seconds someone could be born in. Note we use $366$ as a worst case to raise the number with leap years.
</br>
Since $8000000000>3731443200$, there are more people than possible seconds to be born, therefore at least two people must have been born at the same exact second.
</br>
QED
</details>
{{% /callout %}}

Alright that one was pretty straightforward, but try this problem.

> **Example**: Select $1500$ unique random integers in the range $1\leq x\leq 2022$. Prove there must exist at least one pair of selected numbers that adds up to $2023$. For example if $\\{5,2018,54,\ldots\\}$ were selected, then $5+2018=2023$

{{% callout info %}}
<details>
<summary>Proof</summary>
Lets first start off this proof by thinking of just how many pairs of numbers actually add up to 2023. Well you can have
$$
\begin{align}
1+2022&=2023 \\
2+2021&=2023 \\
3+2020&=2023 \\
&\vdots \\
1011+1012&=2023.
\end{align}
$$
Since these are all the possible pairs that add to $2023$, we can say that we know there are $1011$ possible pairs.
</br>
Notice that if we pick two numbers from the same pair, then we are guaranteed to add to $2023$. Well since we are picking $1500$ numbers, and there are only $1011$ possible pairs, we are going to have to pick at least two numbers from the same pair via the pigeonhole principle as $1500>1011$! Therefore we know we can find a pair.
</br>
QED
</details>
{{% /callout %}}

Maybe that one was easy too for you, but how about I leave this one for you that isn't too hard, but certainly might take a few minutes of thinking.

>  **Example**: You are writing a matchmaking system for a competitive video game. Every player in the game is assigned some integer elo value between $1\leq elo \leq 100$ that represents how skilled a player is. At every moment you are provided a list of $10$ players that are currently queued to play the game, and your boss wants you to create two perfectly balanced teams from this list. We say that two teams are perfectly balanced if the sum of all their elos are exactly equal to each other; note that the two teams may have differing amount of players and you do not need to use every player from the list. For example, given the players $$\left[20, 10, 5, 87, 36, 1, 4, 4, 8, 98\right]$$we can create the teams$$\left[4, 36\right], \left[1,4,5,10,20\right].$$Show that it is possible that given a list of $10$ players you can **always** find two perfectly balanced teams, assuming that any given player can only be on one team.

And then if you think that one is easy, try this cool little theorem

> **Theorem**:  Let $A$ be a finite set and $f:A\rightarrow A$. Define the sequence $$x_0, x_1, x_2, x_3\ldots $$ to be $$ x_0\in A, x_{n+1} = f(x_n).$$ There exists a value of $N$ such that if $n>N$ then $x_n$ is guaranteed to be inside of an infinitely repeating cycle of the sequence. 

---

## Practice Problems

> **No Compression Theorem**: Any compression algorithm that shortens at least one file, must also make at least one file bigger.