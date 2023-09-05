---
title: Direct Proofs
linktitle: Proof Techniques, Direct Proofs
toc: true
type: book
date: 2023-01-06
draft: false
tags:
    - cs241
    - logic
    - proofs

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 115
---

## Why do we prove things?

In mathematics everything needs to be rigorously proven in order for it to be truly accepted and believed as rule. This runs counter to a lot of other fields in which something is to be believed true if there is sufficient evidence supporting it. In physics, you can drop an object a million times and be confident at saying it is a law that a dropped object will fall, in math it does not matter how many examples you have that verify your claim if you can't explain the underlying reason it works; if you want to see why see this thread of [large counter examples](https://math.stackexchange.com/questions/514/conjectures-that-have-been-disproved-with-extremely-large-counterexamples).

This is not strictly for mathematicians either though, computer scientists are held to the same standard. How do you know that your code truly works? You can run some tests cases sure, but if there is some unaccounted for error you might end up costing your company a large amount of money. You need to be able to prove that your code works *all the time*.

At the end of the day, if you don't prove your work in any field [haters will say its fake](https://www.youtube.com/shorts/DlYzYgV5-8c)[^haters_meme] and we all know that there is no greater catharsis than proving some stupid dumbass on the internet wrong.

---

### The Axioms

You might have the thought "why do we need to prove it by writing it down, math is the language of the universe so can't I just provide you evidence and thats good enough?" This is a fair question but misses something big, what is the universe?

No seriously, in mathematics we are working with simplified rules and numbers to make proofs and statements, but how are those rules proved? And how do you prove the rules that prove those rules? And what about the rules that prove those rules? You could keep going deeper and deeper and realize that at some point you will just need to take an assumption as ground truth and work from there. These statements that we assume to be true are called *axioms*, and your choice of axiom completely determines everything about the mathematics you work with.

For example, there are multiple different types of geometry, and some of which have their differences in how parallel lines interact with each other; classically they never cross, but what if they did [^projective_geometry]?

> **Definition**: An **axiom** is a statement that is assumed to be true, generally with the purpose of proving other statements.

For our class, we will not be referencing Axioms very regularly, however if you are curious you could check out several resources that mention the axioms they use, my personal favorite being Terry Tao's [Analysis I](https://www.amazon.com/Analysis-Third-Texts-Readings-Mathematics/dp/9380250649) book.

---

## The Direct Proof

Now that we have explained *why* we prove things, lets actually show how you would go about proving something. The most simple proof that we can do is called the **Direct Proof**, which is the way that you are likely most familiar with proving things to people in speech. We begin by starting with some statement we know to be true, and then continually make steps that we know to be true to bring us to our next true statement. From the *IMPLIES* [truth table](/course/introtologic/sections/logicaloperators/#implies) we know that if $P=1$ and $(P\rightarrow Q)=1$ then the only row that this occurs is $Q=1$. As such we can make chains of statements starting at $P$ that eventually reach to our end goal that we wish to claim.

> **Definition**: A direct proof is a proof that starts with some initial true statement $P$, before moving down a true set of implications $$P\rightarrow P_1\rightarrow P_2 \rightarrow\ldots\rightarrow P_n \rightarrow Q$$ where $P_i$ represents some intermediate step and $Q$ is the end goal.

---

### A Not Mathy Direct Proof

In order to show you that a direct proof is literally just a written out explanation of steps that eventually lead you to an end goal, lets consider a funny little minor example to see what a proof looks like.
> **Example 1**: Prove that if there are no people on an escalator, the escalator is always faster than taking the stairs.
{{% callout info %}}
<details>
  <summary>Proof</summary>
    Imagine that the fastest rate you could climb the stairs was $v$. Since the escalator is just a staircase with no people on it you can run up the escalator at speed $v$. However the escalator is also moving up at speed $v_e$, therefore you are moving up the escalator at speed $v+v_e$. 
    <br/> Since $v_e>0$ in order to move up, we can say that
    $$v < v+v_e$$
    which proves our claim.<br/>
    <b>Q.E.D.</b>
</details>
{{% /callout %}}

You might think *"Wow thats pretty wordy for a math class, don't we deal with numbers?"* to which my answer is that you best get used to the words cause higher level math involves a shitton of proofs and text. Even in a more mathy example we'll find ourselves using a surprising amount of text. Speaking of which

---

### A More Mathy Example

We can't put it off forever and its finally time to do some math. Don't psych yourself out though, just move along in steps of what is true.

> **Example 2**: $(P\rightarrow Q) \equiv (\bar{Q}\rightarrow\bar{P})$, where $\equiv$ means the two expressions are equivalent to each other.
{{% callout info %}}
<details>
  <summary>Proof</summary>
   If two expressions are equivalent, that means that they will always have the same output as each other if given the same input. Since a truth table is literally listing out all the different input output pairs, if the truth tables are equivalent then the expressions are.
    $$\begin{array}{c c|c|c c|c}
    P & Q & P\rightarrow Q & \bar{Q} & \bar{P} & \bar{Q}\rightarrow\bar{P} \\\hline
    0 & 0 & 1 & 1 & 1 & 1 \\
    0 & 1 & 1 & 0 & 1 & 1 \\
    1 & 0 & 0 & 1 & 0 & 0 \\
    1 & 1 & 1 & 0 & 0 & 1
    \end{array},$$
    note that we included intermediate steps to make the work more clear. Since the two truth tables are the same, the expressions are equal.</br>
    <b>Q.E.D.</b>
</details>
{{% /callout %}}

---

### Proof by Contrapositive: A Less Direct, Direct Proof

In the previous section we proved that $\bar{Q}\rightarrow\bar{P}$ was the exact same expression as $P\rightarrow Q$, as such if we want to prove that $P\rightarrow Q$, we are completely allowed to prove $\bar{Q}\rightarrow\bar{P}$ instead.

This is called a *Proof by Contrapositive* as the expression $\bar{Q}\rightarrow\bar{P}$ is called the **contrapositive** of the original expression. While this might seem weird and unique, just know that it's literally just a direct proof.

> **Example 3**: What is the contrapositive of the statement "If it rains, I will cancel my plans"?
{{% callout info %}}
<details>
  <summary>Answer</summary>
    Reading our statement, we can see that $P=$"it is raining" and $Q=$"I cancel my plans" which makes $P\rightarrow Q$ our original statement. To turn this into the contrapositive we flip the order and invert the statements. This gives us "If I don't cancel my plans, then it is not raining".
</details>
{{% /callout %}}

{{% callout warning %}}
Be careful when changing around your statements for proofs, $\bar{P}\rightarrow\bar{Q}$ is **not** equivalent to the contrapositive!
{{% /callout %}}

---

### What the heck is Q.E.D.?

Q.E.D. is an abbreviation of the latin phrase *quod erat demonstrandum* which translates to "what was to be shown". Back in ye-onder days Greek mathematicians would end all their proofs with the phrase as a sort of mathematical mic drop. People eventually shorted it to **Q.E.D.** and have been using it to mention that the proof is complete since.

More modern notation[^qed_meme] has people mostly using a box, either $\square$ or $\blacksquare$ because its easier to write on a board, but you can use any symbol you want. I'm actually quite partial to seeing students complete their proofs with funny stupid little images.

[^projective_geometry]: When you paint railroad tracks that connect off in the distance you literally are working in a [different form of geometry!](https://en.wikipedia.org/wiki/Projective_geometry#History)

[^haters_meme]: Feel free to send memes to add to the pages. If its not funny I'll publicly shame you

[^qed_meme]: **Q.E.D.** is actually infinitely meme-able as it is a phenomenal way to stunt on someone you're fighting with
