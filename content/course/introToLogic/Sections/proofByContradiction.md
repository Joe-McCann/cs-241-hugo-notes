---
title: Proof by Contradiction
linktitle: Proof Techniques, Proof by Contradiction
toc: true
type: book
date: 2023-01-07
draft: false
tags:
    - cs241
    - logic
    - proofs

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 120
---

## Contradiction with Style

### Wait, How the Fuck did we get Here?

Last section we showed that the most obvious way to prove things is to start with something that we know is true, and then take a bunch of true steps from there to get to our eventual claim that we were trying to show. However, that is not the only way to prove something, the analog of a Direct Proof is called *Reductio ad absurdum* which means *"reduction to the absurd"*. Think about a time when you were fighting with someone (parent, sibling, significant other, etc) and they start with some statement which then eventually leads somewhere thats completely absurd. For example, they start with a "you don't put enough effort into us", only to end up at "you hate me!" well wait, what the fuck something is wrong here cause that conclusion is wrong[^girlfriend_fights]!

This is the general jist of reduction to the absurd, if you start with something and eventually end up at something you know is false, then your initial claim has to be false as well. From the *IMPLIES* [truth table](/course/introtologic/sections/logicaloperators/#implies) we can see that if $Q=0$ and $(P\implies Q)=1$ then the only row in which this is true is $P=1$.

{{% callout info %}}
Remember! Proving something false counts as proving the statement!
{{% /callout %}}

A specific funny example that I like comes as a [clip](https://www.youtube.com/watch?v=3LAnmnS0-9g) from the 1996 Comedy Film  __Happy Gilmore__,

> Shooter: You're in big trouble, I eat little pieces of shit like you for breakfast
>
> Gilmore: You eat pieces of shit for breakfast?
>
> Shooter: ... no!

Gilmore points out that Shooter eating literal shit for breakfast is ridiculous and false, so the initial claim that Gilmore should be scared because beating Gilmore is practically routine is also false.

---

### Proving Things, but Indirectly

This absurism stuff is all well and good sure, but what if I don't want to prove some claim is false, what if I want to prove a claim is true? This is where we will use the *indirect proof* which, when used in conjunction with *reductio ad absurdum* is called the __proof by contradiction__. This is by far the most powerful tool that mathematicians have ever used; to this day things are constantly getting proved using contradiction because it is so easy to do. If you can master the Proof by Contradiction, you will be unstoppable in internet debates I guarantee 💯

> __Definition__: A __Proof by Contradiction__ is a proof in which you start with some statement $P$ of unknown truth value and progress with $$P\rightarrow P_1\rightarrow\ldots\rightarrow P_n\rightarrow Q$$ where $Q=0$. Since a true statement can never imply a false statement, by extension $P$ must also be false.

Note that since all implications are true, no intermediate $P_i$ can be the first false one, as if $P$ was true it could never lead to any false $P_i$, as such $P$ *must* be false.

Now what do we do if we want to prove $P$ true by using a proof by contradiction? Well, what we can do is to just prove that $\bar{P}$ is false, because since $\bar{P}$ has the opposite truth value of $P$ then $P$ itself must be true. This is an indirect proof, in which we aren't proving $P=1$, instead we are proving $\bar{P}=0$, which does the job.

Now lets work through the easiest math example such that you can see a proof by contradiction in action.

> __Theorem__ <a name="no_largest_number_theorem">No Largest Number</a>: Let $n$ be an integer, there is no largest $n$ such that for all other integers $m$, $n>m$
<details>
  <summary>Proof</summary>
  Proof: Let us assume that the negation of $P$ is true, and there is in fact a largest integer we will call $n$. We now will consider the integer $n+1$. Since $n$ is the largest number, we know that $n+1 < n$. We can subtract $n$ from both sides to get that $1 < 0$. This is clearly false which has led us to a contradiction, and as such our initial statement that there is a largest number $n$ is false.
  </br>
  <b>Q.E.D.</b>
</details>

[^girlfriend_fights]: I promise this isn't coming from experience 💀
