---
title: Logical Operators
linktitle: Logical Operators
toc: true 
type: book
date: 2023-01-02
draft: false
tags:
    - cs241
    - logic
    - propositions
    - operations

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 210
---

## Remember, you're doing math

Now that we have propositions which are the basic items of true and false, we will now
define operators that we can use to create functions that take in one or more propositions,
and output some truth value that represents the result of "combining" our propositions in different
ways. For those of you familiar with programming (which should be all my students), these operations
should seem very familiar.

---

### NOT

The first and most basic of operations in the operation $NOT$. This operation is pretty
straightforward to how we use the word in our everyday sentences. If we use $NOT$ to negate
a proposition, statement, expression, etc. then we are flipping the truth value. This means that we
are turning a true into a false, and false into true. There is a lot of different notation to represent
this, but if we have some proposition $P$, then we write the negation as any of the following
$$
\bar{P}, \lnot P, not P, !P, \sim P
$$

Personally, I prefer the notation $\bar{P}$, which is what we will be using in this website.
However, a true first-order logic class would use $\lnot P$.

>__Definition__: The __negation__ of a proposition $P$, notated as $\bar{P}$, is the opposite truth value of $P$. $\bar{P}$ is sometimes called the compliment of $P$.

Finally, we can write out what is called a *truth table* for our expression. This is just the
act of listing out all possible inputs with their associated outputs[^1].

{{< math >}}
$$\begin{array}{c|c}
P & \bar{P} \\ \hline
  0 & 1 \\
  1 & 0 \\
\end{array}$$
{{< /math >}}

>*Example 1:* Let $P=$"NJIT is a university in New Jersey". Negate $P$
<details>
  <summary>Answer</summary>
  The negation in "NJIT is not a university in New Jersey", which has the opposite truth value of the original, as the negation is false.
</details>

>*Example 2:* Given $P=1$, what is $\bar{\bar{P}}$??
<details>
  <summary>Answer</summary>
  First we get the $\bar{P}$, which is $0$ since $P=1$. We then negate that which brings us back to the solution, which is $\bar{\bar{P}}=1$
</details>

---

### AND

In our previous operation of $NOT$, we saw that there was one input (which is called a unary operation),and now we will explore
our first operation that takes in two inputs: $AND$. Just as before, we can first consider what this does when we speak in English.
If I was to tell you that I have a cat __and__ I have a dog, you could infer that individually the statements "I have a cat" and "I have a dog"
are both true. In essence that is what $AND$ does, it takes in two truth values, and evaluates to true __if and only if__ both inputs
are true. If $P,Q$ are propositions, notationally we can see a few ways of writing this
$$
    P\cdot Q, P\land Q, PQ, P and Q
$$
For me I will be sticking with the $P\cdot Q$ as it has a nice bit of analog to multiplication, which makes remembering it easier. However
the logical notation is $P\land Q$.

> __Definition__: The $AND$ operation takes in two truth values $P,Q$ and returns true if both $P,Q$ are true, and false in all other cases

The truth table for $AND$ looks as follows, note that now we have two inputs so there are four possible input cases

{{< math >}}
$$\begin{array}{c c |c}
P & Q & P\cdot Q \\ \hline
  0 & 0 & 0 \\
  0 & 1 & 0 \\
  1 & 0 & 0 \\
  1 & 1 & 1 \\
\end{array}$$
{{< /math >}}

>*Example 3:* Let $P=$"CS 241 is a CS class" and $Q=$"NJIT is a NY college". Is $P\cdot Q$ true?
<details>
  <summary>Answer</summary>
  No, it is not true because while $P$ is true, $Q$ is not
</details>

---

### OR

Just like before, this operation that we are creating for ourselves has meaning that we use in our everyday speech. However in this
case our speaking phrases may mislead us, as if I was to tell a student "you can take 10 points extra credit on the final, or a HW assignment"
one could assume that there is implied "either" in there. Theres no way that I'd provide the option for both, so it is assumed that you
can only have one. Rather here we don't care how many of the options are true in an $OR$ statement, just that at least one of them is True.

For example, when I say "I would be happy with an 10 or 20 extra credit points", this means that I'd be happy if I got $10$, if I got $20$, and the (unlikely) case in which I got both $10, 20$. This means the logical $OR$ is "inclusive" and some people on the internet like to make a [dad joke about it](https://www.reddit.com/r/InclusiveOr/)[^2]

Symbolically for two propositions $P,Q$ then we represent $P$ $OR$ $Q$ as
$$
  P\lor Q, P+Q, P or Q, P||Q
$$
and I will use $P+Q$ for this class, again to make things simpler.

> __Definition__: The $OR$ operation takes in two truth values $P,Q$ and returns true at least one of $P,Q$ is true.

The truth table is given as

{{< math >}}
$$\begin{array}{c c |c}
P & Q & P+Q \\ \hline
  0 & 0 & 0 \\
  0 & 1 & 1 \\
  1 & 0 & 1 \\
  1 & 1 & 1 \\
\end{array}$$
{{< /math >}}

>*Example 4:* Let $P=$"CS 241 is a CS class" and $Q=$"NJIT is a NY college". Is $P+Q$ true?
<details>
  <summary>Answer</summary>
    Yes, because even though NJIT is an NJ college, CS 241 is in fact a CS course. 
</details>

---

### IMPLIES

Unlike the previous operations, this is one that you likely have yet to see before. Linguistically, we take two
statements $P,Q$ and return True if the implication "If $P$, then $Q$" is true. This one
has a truth table that is a little counterintuitive, so I will first start by showing the truth table, and then
explaining from there.

{{< math >}}
$$\begin{array}{c c |c}
P & Q & P\rightarrow Q \\ \hline
  0 & 0 & 1 \\
  0 & 1 & 1 \\
  1 & 0 & 0 \\
  1 & 1 & 1 \\
\end{array}$$
{{< /math >}}

By "If $P$, then $Q$" is true, you might assume this means "If $P$ (is true), then $Q$ (is true)", but thats not really the case. In fact, we really only say that this implication is False if the implication leads to a __contradiction__, which is when a true statement leads to a false statement. A good way to think of this is that we will say $P\rightarrow Q = 0$ if the sentence makes someone a "liar". Heres an example to explain whats going on

I have a friend Lake who was teased for flaking always at plans[^3] to the point she was called Flakey Lake. Suppose we make plans to go to a park and she tells us *"If it's not raining, then I won't flake"* where in this case $P=$"It was not raining" and $Q=$"I didn't flake", with $P,Q$ being past tense so we can say this is some next morning flame session or something. Now consider the following cases and remember that $P\rightarrow Q=0$ if Lake is a liar.

1. If $P$ is true and $Q$ is true, then Lake __is not__ a liar because it means that it did not rain and she didn't flake, keeping to her word.
2. If $P$ is true and $Q$ is false, then Lake __is__ a liar because even though it didn't rain she still flaked.
3. If $P$ is false, then regardless of the value of $Q$ Lake is not a liar because he statement __only applied to when it wasn't raining__, she did not say what would happen if it was!

Practically, implications are useful because they form the basis of proofs, for reasons we will later get to. However it's also important to realize that this is just an operator that we defined via a truth table, so we need to be careful to assign meaning to it when sometimes the statement can be nonsense: for example

>*Example 5:* Let $P=$"CS 241 is a CS class" and $Q=$"NJIT is a NJ college". Is $P\rightarrow Q$ true?
<details>
  <summary>Answer</summary>
    Yes it is true. Even though it doesn't really make sense from a linguistic perspective, both $P,Q$ are true which means the implication is true as well.
</details>

---

### IF AND ONLY IF

The final operator that we are going to discuss is one that we've actually used before, but in a way that you probably didn't realize was a logical operator. Rather you just probably thought I was being overdramatic. The operator *if and only if* which for propositions $P,Q$ is notated as
$$
P\iff Q, P iff Q
$$

is defined in terms of other operators. I usually use iff when I'm writing because its so freaking cool holy crap.

> __Definition__: The __if and only if__ operation is defined as $P\iff Q := (P\rightarrow Q)\cdot (Q\rightarrow P)$[^4]

We say this is a bidirectional implication because both $P,Q$ imply each other. When we get to proving claims if you see the statement iff then you know that you actually have to prove __both__ directions of the implication.

*Note: hopefully this section makes sense I'm hella tired right now and am probably speaking nonsense* 😬

Here is the truth table, notice though that this should look familiar ...

{{< math >}}
$$\begin{array}{c c |c}
P & Q & P\iff Q \\ \hline
  0 & 0 & 1 \\
  0 & 1 & 0 \\
  1 & 0 & 0 \\
  1 & 1 & 1 \\
\end{array}$$
{{< /math >}}

This is actually the same truth table as $P=Q$ which is true whenever both propositions hold the same truth value! As such we can say that iff is equivilent to saying that the two statements are the same thing.

[^1]: Later down the line, you will see this is just listing out the table of a function with $\{0,1\}$ as its input and ouput sets (but we're not there yet)

[^2]: Most of the time the joke here is just evaluating the truth of an $OR$ statement, which doesn't really have to do with inclusivity, but occassionally theres a funny one.

[^3]: Whether thats a current habit or in the past is a discussion for another day.

[^4]: Note that $:=$ means "is defined to be". So here we are defining iff to be that expression
