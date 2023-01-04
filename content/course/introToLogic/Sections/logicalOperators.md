---
title: Logical Operators
linktitle: Logical Operators
toc: false 
type: book
date: "01/02/2023"
draft: false

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
    P\cdot Q, P\land Q, PQ
$$
For me I will be sticking with the $P\cdot Q$ as it has a nice bit of analog to multiplication, which makes remembering it easier. However
the logical notation is $P\land Q$.

> __Definition__: The $AND$ operation takes in two truth values $P,Q$ and returns true if both $P,Q$ are true, and false in all other cases

The truth table for $AND$ looks as follows, note that now we have two inputs so there are four possible input cases

[^1]: Later down the line, you will see this is just listing out the table of a function with $\{0,1\}$ as its input and ouput sets (but we're not there yet)
