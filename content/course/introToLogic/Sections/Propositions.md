---
title: Propositions
linktitle: Propositions
toc: true
type: book
date: 2023-01-01
draft: false
tags:
    - cs241
    - logic
    - propositions

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 205
---

## I want the truth!

Since logic is all about the study of true and false, we need some sort of basic type of
object that will represent and store these true and false values. The most basic of this
is called a __proposition.__

> __Definition__: A *proposition* $P$ is an object that has an objectively true or false value

The important part of this definition is the idea that the proposition must have an objectively true
or false value. This means that it cannot be open to interpretation based on who is being asked the
question. Currently you are reading about math, so the statement *"You are reading about math"* would be objectively true,
but *"You enjoy reading about math"* might be a bit more subjective, depending on whether you are a student
of mine who is forced to read this lmfao. The first example would be a proposition, the second would not.

Sometimes we represent this true or false with respect to a variable that represents the truth
of some particular expression. For example, we might say that
$$
P=\left(2+2=4\right)
$$
in which this case $P=True$ since we know that $2+2=4$ is in fact true. Notation wise
true and false are often represented as $T/F$, however we shall use the Computer Science notation
in which $1$ represents true and $0$ represents false.

Expanding away from math for a second, things being true or false is common all across every field; whether
it is a scientist using experiments to justify conjectures, engineers trying to convince their managers that
its a good idea to perform yearly inspections, or you trying to lie to your mom that you didn't get a tattoo[^1].
Compared to our previous math example, these examples are based on the idea of adding propositions into
our sentences and arguments. A proposition that has a linguistic meaning because of words used inside of
it are called __statements__; note that the proposition here is the true or false evaluation of our statement
based on its meaning. This is all much more philosophical than our monkey applied brains really care for,
so for our purposes we will use these terms interchangeably (which we literally did in the first paragraph),
but feel free to check some resources that [explain the difference](https://philosophy.stackexchange.com/a/10896)

---

## You can't handle the truth!

Lets give some example problems to identify if the objects are or aren't propositions, and if they are,
then provide whether they are true or false

---

>*Example 1:* Let $P=$"NJIT is a university in New Jersey". Is $P$ a proposition? If so is it true?
<details>
  <summary>Answer</summary>
  Yes, $P$ is a proposition as it has an objective truth value, of which it is true because NJIT is a school inside of NJ.
</details>

---

>*Example 2:* Let $P=$"The Sun is a planet". Is $P$ a proposition? If so is it true?
<details>
  <summary>Answer</summary>
  Yes, $P$ is a proposition as it has an objective truth value, however this proposition is not true as the sun is a star, not a planet. Don't ask about pluto though 😢
</details>


---

>*Example 3:* Let $P=$"Rain is terrible weather". Is $P$ a proposition? If so is it true?
<details>
  <summary>Answer</summary>
  No, $P$ is not a proposition because its truth might change depending on who is being asked. I personally think the rain is amazing, however there are a non-negligable amount of losers out there who dislike the rain for some reason.
</details>

[^1]: Fun fact, I do have a tattoo. It was an impulse decision and I thought my parents at the time would be surprised, but they literally could not have cared less. 
