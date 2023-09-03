---
title: Balanced Teams - The League of Legends Problem
linktitle: Balanced Teams
toc: true
type: book
date: 2023-01-16
draft: false
tags:
    - combinitorics
    - number theory
    - algorithms

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5000
---

## Problem Description

Let $P$ be a list of players that are queueing up to play a game of *League of Legends*, if you're not a nerd feel free to switch this out to be some other team game like soccer or something. Each player $p_j$ is given an integer value that represents their "skill" in the game $1\leq s_j \leq M$, with $M$ being the maximum skill a player can have. For example, if $M=10$ our list of players could look something like
$$
P = \left[6,1,7,2,6,4,10,5,3,6,8,2,1,3\right].
$$

League is a team game, so we want to form two team of $K$ players where a player can only be on one team. Let the skill level of a team be represented the sum of the individual skills of all the players on that team. Our question becomes: 

> **General Balanced Team Problem**: How many players $n$ do we need to have in $P$ given $K,M$ such that we are always guaranteed to find two distinct teams that have the same skill level? Define the solution $n$ to be $B(M,K)$ for a given $M,K$

> **The League of Legends Problem**: What is the solution to the balanced team problem when $M=100$ and $K=5$?

League of Legends is a $5$ player game, so lets provide an example for $K=5$ in the example given above. We can see
$$
T_1 = \\{p_0,p_1,p_2,p_3,p_4\\} \implies 6+1+7+2+6 = 22,
$$
and
$$
T_2 = \\{p_5,p_6,p_7,p_{11},p_{12}\\} \implies 4+10+5+2+1 = 22
$$
so T_1, and T_2 would be balanced teams. Its not always possible to find a balanced pair of teams for every list, for example when $M=10$ and $K=5$ we can see that
$$
P = \\{1,1,1,1,1,1,1,1,1,2\\}
$$
as one team will have $5$ values of $1$ summing to $5$, but the other team will sum to $6$ as they have a $2$. As such we want to find the size $n$ such that you will always be able to find a balanced team. 

### A Mathy Description

Mathematicians tend to rather pressed about math problems that are not super well defined, so I will do that here. Let $P$ be a list of integers with $0 < P_j \leq M$. We define a "team" to be a set of indices of $P$ called $T$. We define the general balanced team problem to be

> **General Balanced Team Problem**: What is the smallest value $n$ such that given any list $P$ of size $n$ where each $0 < P_j \leq M$, we can find two sets of indices $T_1, T_2$ such that $T_1\cap T_2=\null$ and
$$
\sum_{i\in T_1} P_i = \sum_{i\in T_2} P_i
$$

### General Results

> **Lemma 1**: Given $M,K$, $B(M,K) \leq M\cdot (2k-1) +1$
{{% callout info %}}
<details>
<summary>Proof</summary>
This is easily proved by the pigeonhole principle as if any number occurs $2k$ times we can find two balanced teams by just taking only that number in each team. As such we can put $(2k-1)$ of each number into our list giving us $M(2k-1)$ items. The next item is guaranteed to create a balanced team. As such $n\leq M(2k-1)+1$.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

---

## The League of Legends Problem $M=100,K=5$

Extrapolating to any value of $M,K$ seems to be really freaking hard, so we'll focus our efforts on specific cases that are fun to work with and work on. We can lower the bound in increments for the League of Legends problem which is $M=100, K=5$. 

By Lemma $1$ we know that $B(100, 5)\leq 901$

We can also show that $n\leq 101$ by clever application of pigeonhole principle
> **Lemma 2**: Given $M=100,K=5$, $B(100, 5) \leq  101$
{{% callout info %}}
<details>
<summary>Proof</summary>
Since $101 > 100$, by pigeonhole principle, we know that there must be at least two of the same numbers in the list, take those two numbers and put them in differing teams. Our problem is now equivalent to finding two equal teams of $K=4$ with $n=99$. Note that if we had $99$ unqiue numbers, then we could group up the numbers into pairs that have equal sums $(1,100),(2,99),\ldots,(50,51)$ we can take two of each of these equal pairs onto a team. As such, if we were to want to avoid creating two balanced teams, we would have to double up some numbers.
</br>
</br>
Notice though that if we have $4$ values that include duplicates, we could just put one of each of those values on a team and create two balanced teams from there. In fact, in order to minimize the number of unique values we have, we would have one specific value with $7$ copies, as that would not be enough to split into two teams, however that would still give us at least $92$ unqiue numbers which is guaranteed to give us $4$ balanced teams. 
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

By a cute little corollary we can see that

> **Corollary**: When $M=100$ and $K=4$ then $B(100,4)\leq 60$

which we get by just using the same argument we had before.

Computationally on MathOverflow, it was shown that for the League Problem $n > 17$ and the user who discovered that actually conjectured that $n=18$.

---

## Simple Cases

Its always worthwhile consider simple cases and build up from there, so I wonder if it's worthwhile to solve recursively the same way that I did for the probability number guesser problem.

### $K=1$, $M=1$

If we have that there is only one player per team, then it doesn't matter what the value of $M$ is we can actually solve for it

> **Lemma 3**: If $K=1$ then $B(M,1)=M+1$
{{% callout info %}}
<details>
<summary>Proof</summary>
Since we have only one player per team, then the only way to get equal teams is when each team has a player of equal value. In order to guarantee this we use the pigeonhole principle to see that we need $M+1$ items.  
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

> **Lemma 4**: If $M=1$ then $B(1,K)=2K$
{{% callout info %}}
<details>
<summary>Proof</summary>
In order to build our teams in this case, we just need enough players to fill out each team as all players are equal skilled.
</br>
<b>Q.E.D.</b>
</details>
{{% /callout %}}

### $K=2$

Lets just try to work through the problem when $K=2$ for various cases. I think there are different situations when $M < K$ compared to the other way around sooooo lets just grind. This is kinda like a working page I'm just going to type my thoughts.
$$
B(1,2) = 4
$$
by Lemma $4$. 
$$
B(2,2) = 5
$$
as if we could have one value containing $3$ and one value containing $1$ such as $[1,1,1,2]$. Adding any value to this will create a solution.
$$
B(3,2) = 6
$$
as $[1,1,1,3,2]$ has no solution and we can get a solution by adding any number. Ok lets think a bit. Lets think of how we can be building these lists without teams. If I have a list that contains $u$ *unique* numbers such that we have no solution what can we do. 

Oh shit I think I figured it out. Let $U$ be the largest list where all the numbers inside of it are unique and there are no pairs that sum to the same item. If we take the smallest item $u$ of $U$ and add $2$ copies of it then there will still be no balanced teams, as the sum $u+x$ for every other $x\in U$ must have only one occurrance, and $u+u$ cannot be the result of any other two numbers as $u$ is minimal. Also, this must be maximal as if we had any other smaller set of unique items, adding $2$ more items would create a solution or be just smaller. So we actually can get a "closed form" solution for whenever $K=2$, because adding one more item on top of this solves the problem.

> **Definition**: Let $S(i)=n$ be the largest amount of items such that there exists an $S$ with $1\leq x_1 < x_2 < \ldots < x_n \leq i$ with $x_j\in S$, such that no pairs of items $x,y\in S$ share a sum with any other pair of items.

Then we have the solution to $B(M,K)=S(M)+3$, and the maximal list that has no balanced teams is $S(M)+2$