---
title: Quantifiers and Notation
linktitle: Quantifiers and Notation
toc: true
type: book
date: 2023-01-07
draft: false
tags:
    - cs241
    - logic
    - proofs
    - quantifiers

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 125
---

## Simple Notation

Before we get into the discussion of quantifiers, lets take a little breather to show some math notation and say what it means. The crazy thing is that mathematicians are lazy af and will take the opportunity to write things in shorthand. Here are some symbols that I use every now and then

1. $\therefore$ - Therefore
2. $P(x)$ - A statement whos truth depends on an input $x$
3. $:=$ - Defined to be
4. $wlog$ - Without loss of generality
5. $\because$ - Because[^because]
6. $s.t.$ - Such that
7. "$P$ is *sufficient* for Q" - If $P$ true, then $Q$ is true
8. "$P$ is *necessary* for Q" - If $P$ is false, then $Q$ is false
9. $|$ - Such that
10. $x|y$ - $y$ is "divisible" by $x$[^divisible]

Hold your horses wait what, that symbol $|$ appears twice there? How am I supposed to know which one is being referred to? This is called *abuse of notation* when the notation can get really obscured and confusing, but sadly the answer is you need to figure it out from the context. In this website I will try to make the notation as clear and explained as possible.

---

## How Much Truth Are We Talking?

When it comes to statements and proofs, you might have noticed that we don't always talk about just a singular statement like *"It is raining outside"* or *"The number 5 is an odd number"*, rather we talk about things that are a little more general or sweeping. For example, we might rather want to say something like *"It rains at least once a month"*, or *"All odd numbers are not divisible by 2"* which provide us insights into just how much truth we can expect from a certain statement.

In particular we often care about two specific types of quantities.

---


### Everything

The first type of quantifier that we will discuss is the __universal quantifier__ which is given by the symbol $\forall$. This quantifier represents the words of *for all* or *every*, in which the statement makes some sweeping generalization. From the above section we have an example of this

> __Example 1__: Every odd number $x$ is not divisible by $2$.

This tells us that given any odd number, we always know that this is a quality about it. Similarly we can say this statement

> __Example 2__: Every student at NJIT has a UCID.

If you provide me with a student from NJIT, they are going to have a UCID, no matter who the student is. To write these statements out in mathematical notation, we can substitute the symbols in where necessary, but a lot of times authors will just write it out to avoid confusing readers. One final example

> __Example 3__: A student at NJIT must pass every required course in order to graduate.

---


### Something

What happens though if we don't care about something being true all the time, but instead we care about it being true sometimes, or more specifically *at least once*? In that case we will use the __existential quantifier__, which besides having a funny name, is one that is used quite frequently in everyday talk and, by extension, mathematics. Very often the most important thing is the existence of a solution, which is something that we will use a lot in later lessons.

Notationally, *"there exists"* is given as the symbol $\exists$. We provide the examples

> __Example 4__: There exists a positive number less that $10$

Note here that we are not saying every positive number is less than $10$, just that we can find at least one that satisfies this condition. Clearly the statement is true as we can give $1,2,3,\ldots,8,9$.

> __Example 5__: There exists a dorm on NJIT campus that is not named after a tree

This statement is also true, in fact there is exactly one dorm on the NJIT campus that is not named after a tree, the honors dorm.

---


### How to Prove Quantified Statements

Generally, you will just use the same techniques that we gove over in class for other problems, Direct Proof, Contradiction, Induction, etc. However, there is a special consideration that you need to make when you are proving quantified statements: you need to prove their quantity. This might sound literally obvious and you might think *"yeah no shit Joe you're just telling me to prove the statement"*, but for the amount of students who miss the mark I feel the need to dedicate this special section to them.

If you want to prove an existence statement, that is super easy. All you need to do is just show that an example exists and you're good. For example if we wanted to prove

> __Example 6__: There exists a student at NJIT with a UCID

I could just select any of my students, ask them for their UCID, and that would prove the claim. However, if we instead wanted to prove a universal statement such as __Example 2__ from above

> __Example 2__: Every student at NJIT has a UCID.

It is __not enough__ to just show that the students in my class have UCIDs. It would not be enough to stand outside all day and ask students if they had UCIDs. You either need to show that *every single student* on campus has a UCID by asking all of them, or by providing some argument about the properties of a student that guarantees that they will have a UCID, such as a document from NJIT that says all students will be given a UCID.

While the first type of argument (exhausting all possible cases) might seem nice, since its easy, this proof will often not work as many times their are infinite cases. As such trying to prove a statement by just showing a bunch of cases are true is often not enough[^1].

---


### Opposites of Quantified Statements

You might be thinking *"hey Joe, why isn't there a symbol or section for something not existing? Seems like that would be useful"* to which you would be right, that would be useful. Don't worry though, because we already have all the necessary tools to write out a statement like this! For notational sake let $P(x)$ be our statement that depends on $x$.

We could write the following using mathematical notation

> __Example 7__: For every value of $x$, $P(x)$ is true
<details>
  <summary>Answer</summary>
    $$\forall x, P(x)=1$$
</details>

> __Example 8__: There exists a value of $x$ such that $P(x)$ is true
<details>
  <summary>Answer</summary>
    $$\exists x, s.t. P(x)=1$$
</details>

These are straightforward enough, but what if I want to negate the second statement to say that there does not exist an $x$ for which $P(x)$ is true? Well is that not just the same as saying $P(x)$ is false for every $x$? So if we want to negate our __Example 8__ we get that

> __Example 9__: Negate __Example 8__
<details>
  <summary>Answer</summary>
    Since saying "There does not exist a case this is true" is the same as saying "This is always false", we have
    $$\forall x, \bar{P}(x)=1$$
</details>

The same actually holds for negating $\forall$, if we want to show that it is not the case that $P(x)$ is always true, we instead just need to show that we can find an example in which $P(x)$ is not true!

> __Example 10__: Negate __Example 7__
<details>
  <summary>Answer</summary>
    Since saying "This is not always true" is the same as saying "There exists a case this is false", we have
    $$\exists x, s.t. \bar{P}(x)=1$$
</details>

---

### Nested Quantifiers

We've actually seen in the previous page with [__No Largest Number Theorem__](/course/introtologic/sections/proofbycontradiction#no_largest_number_theorem) that sometimes we need more than one quantifier. The theorem "there is no largest number" states that for every integer $n$, there exists another number $m$ that is bigger than $n$, written out we get that
$$
\forall n \exists m, s.t. m>n.
$$

In this case we are saying that if we select an integer $n$, we can always find an associated $m$ that is larger, which is true. However, if we switch the order of the quantifiers we end up with a completely different statement
$$
\exists m \forall n, s.t. m > n.
$$
This says that "There exists a $m$, for which all $n$ satisfies $m>n$", which is in fact the statement that there *does* exist a largest number which we know to be false! You can imagine this as we are first locking in a choice of $m$ and then looping through all possible $n$, compared to the first case where we looped through $n$ and found associated $m$.

In general, switching the order of nested quantifiers changes the statement, so its not a simple "stays the same" or "negates it", it changes based on the statement itself. Theres a good explanation [here](https://math.stackexchange.com/questions/304217/is-forall-x-exists-y-qx-y-the-same-as-exists-y-forall-x-qx-y) that uses the same example I gave.

{{% callout info %}}
To negate a series of nested quantifiers, you perform the same steps as negating a single quantifier: flip the quantifiers, and negate the statement.
{{% /callout %}}

[^divisible]: We haven't talked about the idea of divisibility yet, I'm just putting here as an example of abused notation.

[^because]: Not frequently used, just putting it here just in case

[^1]: This goes for general software engineering too, if you test your code on like $5$ inputs, are you sure it works? You need to be able to prove that it *always* works.
