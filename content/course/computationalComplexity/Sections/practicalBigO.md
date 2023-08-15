---
title: What Do Software Engineers Mean By Big O?
linktitle: Practical Big O
toc: true
type: book
date: 2023-07-03
draft: false
tags:
    - cs241
    - software engineering
    - computational complexity
    - asymptotics
    - big O

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 310
---

## Hijacked Notation

Like many things in computer science, Big $O$ has a very specific definition and has a use case designed for number theory. Computer Scientists then took that notation, changed nothing about the notation, and then proceeded to use it in whatever way they felt was convenient[^1].

The way that we are going to do this is that I am going to explain to you how computer scientists tend to use the notation, and then I will explain in the next page what the true technical definition is. To make things (hopefully) less confusing, when I refer to the *true tecnical* definition of notation, I will use the superscript $T$. For example, just using Big $O$ in the colloquial computer scientist version I will write $\mathcal{O}(n)$, but when I wanna talk about the technical definition, I will say $\mathcal{O}^T(n)$. **For your problems, you do not need to do this**, however I will do it so it doesn't make shit confusing and keep you guessing about what I mean. Similarly, when I talk about average and ammortized time I'll talk about different notation for that as well.

### \#Ratio'd

Lets look at some operations functions and put them into a table
$$
$$\begin{array}{c|c|c|c|c}
n & 6n-5 & n+20 & n^2-n+2 & 8n^2+6 \\\\ \hline
1 & 1 & 21 & 2 & 14 \\\\
10 & 55 & 30 & 92 & 806 \\\\
100 & 595 & 120 & 9902 & 80006 \\\\
1000 & 5995 & 1020 & 999002 & 8000006
\end{array}$$
$$

It might not be immediately apparent that there is a pattern here (apart from just plugging shit into a function), but we can make an observation. Lets take the columns of $6n-5$ and $n+20$ and take the ratio of these two functions as $n$ gets bigger and bigger. This might seem kind of random, but it has a real application in that we can use it to compare the two in terms of runtimes. Lets look
$$
\begin{align}
\frac{1}{21} &= .0476\ldots \\\\
\frac{55}{30} &= 1.8333\ldots \\\\
\frac{595}{120} &= 4.9583\ldots \\\\
\frac{5995}{1020} &= 5.87745\ldots \\\\
\end{align}
$$
and if we were to continue this, we would find that our ratio is getting closer and closer to $6$ as $n$ gets bigger and bigger! In a practical sense, we would then say that the code with operations function $6n-5$ should take about $6$ times as long to run as the code that takes $n+20$ operations. So as $n$ grows larger, even though the $6n-5$ function will take longer, we know that it will remain within a ratio no matter how big our input gets. 

Similarly, if we check $n^2-n+2$ and $8n^2+6$ we can see that
$$
\begin{align}
\frac{14}{2} &= 7 \\\\
\frac{806}{92} &= 8.7608\ldots \\\\
\frac{80006}{9902} &= 8.079\ldots \\\\
\frac{8000006}{999002} &= 8.0079\ldots \\\\
\end{align}
$$
in this case the ratio appears to be getting closer and closer to $8$!

However, lets consider the ratio between $n+20$ and $8n^2+6$.
$$
\begin{align}
\frac{14}{21} &= .666 \\\\
\frac{806}{30} &= 26.866\ldots \\\\
\frac{80006}{120} &= 666.7166\ldots \\\\
\frac{8000006}{1020} &= 7843.1431\ldots \\\\
\end{align}
$$
This is werid, it seems like there is no ratio, and as $n$ keeps getting bigger and bigger that the ratio also gets bigger and bigger. This shows us that even as our input grows larger $n+20$ will take longer to run, but the amount of time $8n^2+6$ takes to run is so big in comparison that we can't even find a number to relate the two of them!!

As such, we could almost imagine $6n-5$ and $n+20$ are in some sort of a "class" of functions that all share this common ratio, while $n^2-n+2$ and $8n^2+6$ would be in a different class.

This is the general idea of a software engineer's version of Big $O$, we are trying to group the functions together into classes that share common ratios. 

{{% callout warning %}}
Notice that we are not saying anything about which function at any point is the fastest, just relating ratios as $n$ gets larger and larger. In fact, when $n=1$ the function $n+20$ ran the slowest! As such you shouldn't make any speed claims about specific inputs based solely on Big $O$ information.
{{% /callout %}}

---

## The Computer Scientist's Playbook

For the following sections, we will use this information for our problems, just so we don't need to keep referencing it over and over and over again. Let $B(n)\leq T(n)\leq W(n)$ be strictly positive operations functions, with $B(n),W(n)$ being the best case and worst case functions. Let $n$ represent the "size" of a given input[^2].

Remember, these are not the true technical definitions, rather a way that computer scientists and software engineers generally use them.

### The Biggest of $O$s ⭕

Before we were saying that we want to be able to see what functions share a ratio as $n$ grows large. In order to do this we need *limits* from calculus! Our idea is that we are going to group all the functions that have this common ratio together into a set of functions.

Big $O$ also is specifically referring to the **worst case** scenario for operations, which is why its so popular! When people want to know how long something is going to take, they don't want to hear the $1$ perfect case where it runs instantly, they want to know how bad can it get!

> **Definition**: We say that function $T(n)\in\mathcal{O}(f(n))$ if the worst case function $W(n)$ satisfies $$0<\lim_{n\rightarrow\infty} \frac{W(n)}{f(n)}<\infty.$$

What this definition is saying, is that we are creating this set $\mathcal{O}(f(n))$ and placing functions inside of it who's *worst case* operations functions have a ratio limit with $f(n)$ that is not $0$ or $\infty$

We also can define a relation $\sim$ to relate functions together and make this notation easier

> **Definition**: We say that two functions $f(n)\sim g(n)$ iff $$0<\lim_{n\rightarrow\infty} \frac{f(n)}{g(n)}<\infty$$

This is a way of saying that the two functions grow at a comparable rate, and we can define our set $\mathcal{O}(f(n))$ to be the set of all operations functions $T(n)$ whos worst case $W(n)\sim f(n)$!

Now lets do some examples

> **Example**: Prove that $4n+5\in\mathcal{O}(2n)$ by showing $4n+5\sim 2n$

{{% callout info %}}
<details>
<summary>Proof</summary>
Using L'Hopital's Rule
$$
\lim_{n\rightarrow\infty}\frac{4n+5}{2n} = \lim_{n\rightarrow\infty}\frac{4}{2} = 2
$$
Since $0 < 2 < \infty$ we know that $4n+5\sim 2n$ and thus $4n+5\in\mathcal{O}(2n)$.
</br>
QED
</details>
{{% /callout %}}

> **Example**: Prove that bubble sort is $\mathcal{O}(n^2)$ using the worst case $W(n)=8n^2+6$ from the previous page
{{% callout info %}}
<details>
<summary>Proof</summary>
Dividing out
$$
\lim_{n\rightarrow\infty}\frac{8n^2+6}{n^2} = \lim_{n\rightarrow\infty}8+\frac{6}{n^2}=8
$$
Which we can do because $\frac{6}{n^2}$ goes to $0$ as $n$ gets large. Since $0 < 8 < \infty$ we know that $8n^2+6\sim n^2$ so $T(n)\in\mathcal{O}(n^2)$ for bubble sort!  
</br>
QED
</details>
{{% /callout %}}

### The Biggest of Omegas

Notice before that Big $O$ represented the *worst case* scenario for us, however we didn't only have a worst case, we also had a best case scenario too! That is the idea of Big $\Omega$, its literally the same as Big $O$ but for best case instead of worst case!

> **Definition**: We say that function $T(n)\in\Omega(f(n))$ if the best case function $B(n)$ satisfies $$0<\lim_{n\rightarrow\infty} \frac{B(n)}{f(n)}<\infty.$$ Also defined as $B(n)\sim f(n)$

Since this is pretty much the same as Big $O$, we can show examples in the same way

> **Example**: Prove that insertion sort is $\Omega(n^2)$ using the best case $B(n)=11n-5$ from the previous page
{{% callout info %}}
<details>
<summary>Proof</summary>
Dividing out
$$
\lim_{n\rightarrow\infty}\frac{11n-5}{n} = \lim_{n\rightarrow\infty}11-\frac{5}{n}=11
$$
Which we can do because $\frac{5}{n}$ goes to $0$ as $n$ gets large. Since $0 < 11 < \infty$ we know that $11n-5\sim n$ so $T(n)\in\Omega(n^2)$ for insertion sort!  
</br>
QED
</details>
{{% /callout %}}

As you might have observed, that proof is a direct copy and paste of the previous proof from Big $O$, which makes sense because they are both pretty much the same thing, just adjusting the best and worst case.

### The Biggest of Thetas

Notice how for insetion sort, we found that our implementation was $\Omega(n)$ but our worst case was $\mathcal{O}(n^2)$. Depending on our input we could have wildly different runtimes that are complete orders different from each other!

However, for selection sort, we found that it was $\Omega(n^2)$ *and* $\mathcal{O}(n^2)$ (I urge you to try to check these all yourself), which means that in a way, we consistently know how we can expect our function to behave for large inputs. In fact, both the best case and the worst case will grow within a constant ratio of each other! This special situation warrants the notation of Big $\Theta$

> **Definition**: An operations function $T(n)\in\Theta(f(n))$ iff $T(n)\in\Omega(f(n))$ and $T(n)\in\mathcal{O}(f(n))$. Also can be shown that $B(n)\sim f(n)$ and $W(n)\sim f(n)$

Based on our previous sorting functions, we can see that our implementation of Bubble Sort is $\Theta(n^2)$, and selection sort is too. Since Insertion sort has a different function for $\Omega, \mathcal{O}$, it doesn't have a $\Theta$ function.

---

## Properties of $f(n)\sim g(n)$

In this section we will review and prove some commonly discussed properties of the $\sim$ relation of functions that we defined earlier. Since $\sim$ is used for both Big $O$ and $\Omega$, its better to talk about it than either of these alone, but the examples we will be using are going to be Big $O$.

Now also, for the following sections we are going to assume that $f(n)$ is a "simple" function that we can write out, such as polynomials or exponentials or logs. Now I put "simple" because remember that this definition of Big $O$ is our practical definition and has some loose questions around it. For example, you might say, *"hey wait, why did insertion sort have no $\Theta$ when it could have just been $\Theta(T(n))$?"* which is a fair question, as the limit of $\frac{T(n)}{T(n)}$ is $1$.

The boring answer is that having $\Theta(T(n))$ is kinda a useless metric that tells us nothing if we cannot easily express $T(n)$ without a range, so we ignore it and focus on the nice functions that surround it instead.

### Properties of the Relation

From our previous chapter on relations, we will analyze three key properties of the $\sim$ relation.

> **Lemma**: For all $f(n)$, $f(n)\sim f(n)$. This means $\sim$ is reflexive.
{{% callout info %}}
<details>
<summary>Proof</summary>
For any value of $f(n)$
$$
\lim_{n\rightarrow\infty}\frac{f(n)}{f(n)}=1
$$
which means $f(n)\sim f(n)$
</br>
QED
</details>
{{% /callout %}}

By extension, this means that every function $f(n)\in\mathcal{O}(f(n))$ and $f(n)\in\Omega(f(n))$ meaning $f(n)\in\Theta(f(n))$. For example, we now can say without doing any extra math that $5\in\mathcal{O}(5)$, or $2^n+5n\in\mathcal{O}(2^n+5n)$, or $n^2-2\in\mathcal{O}(n^2-2)$. Same can be said for $\Omega$ and $\Theta$.

> **Lemma**: For every $f(n),g(n)$, if $f(n)\sim g(n)$ then $g(n)\sim f(n)$. This also means $\sim$ is symmetric.
{{% callout info %}}
<details>
<summary>Proof</summary>
For any value of $f(n),g(n)$, if we know that $f(n)\sim g(n)$ we know that
$$
\lim_{n\rightarrow\infty}\frac{f(n)}{g(n)}=k
$$
where $k\neq 0$ and $k\neq\infty$. As such we can say then
$$
\lim_{n\rightarrow\infty}\frac{g(n)}{f(n)}=\frac{1}{k}
$$
which means $g(n)\sim f(n)$
</br>
QED
</details>
{{% /callout %}}

Now we can see that we can swap function orders around, so if I show that $4n+5\sim n$ this means that not only is $4n+5\in\mathcal{O}(n)$ but also $n\in\mathcal{O}(4n+5)$. As before, this is the same for $\Omega$ and $\Theta$.

> **Lemma**: For all $f(n),g(n),h(n)$, if $f(n)\sim g(n)$ and $g(n)\sim h(n)$ then $f(n)\sim h(n)$. This means $\sim$ is transitive.
{{% callout info %}}
<details>
<summary>Proof</summary>
For any value of $f(n),g(n)$, if we know that $f(n)\sim g(n)$ and $g(n)\sim h(n)$ we know that
$$
\lim_{n\rightarrow\infty}\frac{f(n)}{g(n)}=k_1,
$$
and 
$$
\lim_{n\rightarrow\infty}\frac{g(n)}{h(n)}=k_2,
$$
where $k_1,k_2\neq 0$ and $k_1,k_2\neq\infty$. As such we can say then
$$
\lim_{n\rightarrow\infty}\frac{f(n)}{h(n)}=\lim_{n\rightarrow\infty}\frac{\frac{f(n)}{g(n)}}{\frac{h(n)}{g(n)}}=\frac{k_1}{\frac{1}{k_2}}=k_1k_2
$$
which means $f(n)\sim h(n)$
</br>
QED
</details>
{{% /callout %}}

With these three properties we get the following theorem for free

> **Theorem**: $f(n)\sim g(n)$ is an equivalence relation.

Which is really nice, because it means now that when we are grouping all these functions together that they are creating equivalence classes of functions that are all within a ratio of each other. It will also make some of the following properties easily combined together.

### Tips and Tricks

In this section we will prove some of the commonly sited "tips" of Big $O$ notation and show why you can do them in the practical sense. Fun fact, these properties are also true for the technical definition but I won't be proving those.

> **Theorem**: For any function $f(n)$ and constant $c>0$, then $cf(n)\sim f(n)$. This means that you can "drop the coefficients"
{{% callout info %}}
<details>
<summary>Proof</summary>
For any value of $f(n)$ and $c>0$, 
$$
\lim_{n\rightarrow\infty}\frac{cf(n)}{f(n)}=c,
$$
which means $cf(n)\sim f(n)$
</br>
QED
</details>
{{% /callout %}}

From this we can easily now say things like $2n\in\mathcal{O}(n)$ and $20n^2\in\mathcal{O}(n^2)$, and so on. By transitivity, we also can see that we can always swap constants too, so $2n\in\mathcal{O}(20n)$ as well.

> **Theorem**: For all $f(n),g(n)$, if $$\lim_{n\rightarrow\infty}\frac{g(n)}{f(n)}=0,$$ then $f(n)+g(n)\sim f(n). This means that we can "drop lower order terms".
{{% callout info %}}
<details>
<summary>Proof</summary>
For any value of $f(n),g(n)$ satisfying the properties, 
$$
\lim_{n\rightarrow\infty}\frac{f(n)+g(n)}{f(n)}=\lim_{n\rightarrow\infty}1+\frac{g(n)}{f(n)}=1,
$$
which means $f(n)+g(n)\sim f(n)$
</br>
QED
</details>
{{% /callout %}}

From here now we can see things like $n^2+5n\in\mathcal{O}(n^2)$ without having to do large amounts of calculations, so long as we can easily compare terms against each other.

> **Theorem**: Let $f(n)\sim a(n)$ and $g(n)\sim b(n)$, then $f(n)g(n)\sim a(n)b(n)$. This means that you can multiply the order of the inside of loops with how many times they run.
{{% callout info %}}
<details>
<summary>Proof</summary>
Given the information from the above problem
$$
\lim_{n\rightarrow\infty}\frac{f(n)g(n)}{a(n)b(n)}=\lim_{n\rightarrow\infty}\frac{f(n)}{a(n)}\frac{g(n)}{b(n)}=k_1k_2,
$$
which means $f(n)g(n)\sim a(n)b(n)$
</br>
QED
</details>
{{% /callout %}}

With this, we can see that if we have a function that takes $\mathcal{O}(n^2)$ work and runs $\mathcal{O}(n)$ times, we can expect $\mathcal{O}(n^3)$ work in total.

---

## Little $o$ and little $\omega$

So far we have discussed functions that can share a common ratio with each other, however what about functions that are not comparable with $\sim$ and Big $O$? For example, how can I compare $n$ and $n^2$ as $n$ grows large? Clearly we want some way of saying that as $n$ gets bigger, $n^2$ grows way, way faster.

We can refer to this with Little-$o$ notation, which isn't explicitely used in software, but is implicitely done when people compare functions that are "better" or "worse" than others. 

In a shocking twist, the technical and practical definitions are the same.

> **Definition**: Let $T(n)$ be an operations function. $T(n)\in o(f(n))$ iff $$\lim_{n\rightarrow\infty}\frac{T(n)}{f(n)}=0.$$ Also notated as $T(n) << f(n)$

This tells us that the function $T(n)$ shares no ratio with $f(n)$, and as we increase $n$ $f(n)$ will get very big very quickly comparatively. We also by extension have little $\omega$

> **Definition**: Let $T(n)$ be an operations function. $T(n)\in \omega(f(n))$ iff $$\lim_{n\rightarrow\infty}\frac{f(n)}{T(n)}=0.$$ Also notated as $T(n) >> f(n)$ 

Which we can see means that $f(n)$ as a lower bound is on a completely different order.

To make things easier, lets show that $o(f(n))$ is transitive which will make things easier for us in the future.

> **Theorem**: Let $f(n)\in o(g(n))$ and $g(n)\in o(h(n))$, then $f(n)\in o(h(n))$
{{% callout info %}}
<details>
<summary>Proof</summary>
Given the information from the above problem
$$
\lim_{n\rightarrow\infty}\frac{f(n)}{h(n)}=\lim_{n\rightarrow\infty}\frac{f(n)}{g(n)}\frac{h(n)}{g(n)}=0,
$$
we know this as the numerator goes to $0$ and the denomenator goes to $\infty$. As such $f(n)\in o(h(n))$
</br>
QED
</details>
{{% /callout %}}

This will make ordering functions easier in the future.

### Ordering Common Software Orders

Using Little-$o$ notation, we now have the ability to order common functions in software engineering to see what runtimes are faster than other runtimes.

> **Example**: $n\in o(n^2)$
{{% callout info %}}
<details>
<summary>Proof</summary>
$$
\lim_{n\rightarrow\infty}\frac{n}{n^2}=\lim_{n\rightarrow\infty}\frac{1}{n}=0,
$$
as such $n\in o(n^2)
</br>
QED
</details>
{{% /callout %}}

From this we can say that $n << n^2$ as $n$ approaches infinity. 

Now that we got an easy example out of the way, lets go something more complicated

> **Example**: If $p < q$ then $n^p\in o(n^q)$

Just as an example, this tells us that $n << n^2$, and $\sqrt{n} << n^{.7} << n << n^{2.1}$. We can also use this for fun stuff like showing that $n^{-2} << n^{0}$, even though the idea of $\frac{1}{n^2}$ doesn't make any sense from an operations perspective, its useful in other areas of math.
{{% callout info %}}
<details>
<summary>Proof</summary>
$$
\lim_{n\rightarrow\infty}\frac{n^p}{n^q}=\lim_{n\rightarrow\infty}\frac{1}{n^{q-p}}=0,
$$
which we know $q-p>0$ since $q>p$.
</br>
QED
</details>
{{% /callout %}}

Well shit that's easy, now we have a super simple class of functions that we know how to order. Next up lets show exponential functions like $e^n$ and $2^n$

> **Example**: If $p < q$ then $p^n\in o(q^n)$

Which tells us that $1.1^n << 2^n << e^n << 20^n$ so on and so forth. The proof is quite easy
{{% callout info %}}
<details>
<summary>Proof</summary>
$$
\lim_{n\rightarrow\infty}\frac{p^n}{q^n}=\lim_{n\rightarrow\infty}\left(\frac{p}{q}}\right)^n=0,
$$
which we know since $0 < \frac{p}{q} < 1$ since $q>p$.
</br>
QED
</details>
{{% /callout %}}

Now for our first super interesting one, because we have polynomials and we have exponentials, so at what point do they mix? How big of a polynomial do I need before its worse that $2^n$ for example? Well we have the following

> **Example**: Given $p,q$ with $q>1$, $n^p\in o(q^n)$
This is quite interesting, as it says that no matter how small your exponential is, and how big your polynomial is, eventually the exponential will overtake. 
{{% callout info %}}
<details>
<summary>Proof</summary>
$$
\lim_{n\rightarrow\infty}\frac{n^p}{q^n}=\lim_{n\rightarrow\infty}\frac{p\cdot n^{p-1}}{q^n\log(q)}}=0,
$$
which we know because we can keep iterating this process until eventually the power of the polynomial $n^p$ reduces down to a value that is $\leq 0$, and the exponential term will remain in the bottom.
</br>
QED
</details>
{{% /callout %}}

Now this doesn't tell you at what point the exponential is worse. For example, plugging into wolfram alpha you can see that for $n^{1000}$ vs $1.1^n$, the exponential is actually better until $n=123,000$, at which point the polynomial is faster. $123,000^{1000}$ is a number with over $5000$ digits though, so for all practical inputs, the exponential function would actually perform better!

Regardless, now we know that
$$
\sqrt{n} << n^{.7} << n << n^{2.1} << 1.1^n << 2^n << e^n << 20^n
$$

Finally, I will show a very interesting result that should put into perspective just how *good* certain algorithms are!

> **Example**: Given $p$ with $p>0$, $\log(n)\in o(n^p)$

This is showing us that the function $\log(n)$ grows slower than literally every possible polynomial function, no matter how close to $0$ you go! Note btw that $\log_a(n)=\frac{1}{\log(a)}\log(n)\implies \log_a(n)\in\mathcal{O}(\log(n))$, so it doesn't matter what base of log you use. For us we will use the *natural log* or "log base $e$" which is notated as $\log(n)$. I know you've probably seen $\ln$ used for the natural log, but mathematicians don't use this notation so I won't either.  

{{% callout info %}}
<details>
<summary>Proof</summary>
$$
\lim_{n\rightarrow\infty}\frac{\log(n)}{n^p}=\lim_{n\rightarrow\infty}\frac{\frac{1}{n}}{pn^{p-1}}}=\lim_{n\rightarrow\infty}\frac{1}{pn^{p}}}=0.
$$
Since $p>0$ we know this is true. Therefore $\log(n)\in o(n^p)$
</br>
QED
</details>
{{% /callout %}}

With this we also can get information about things like $n\log(n)$, which is smaller than every polynomial power $>1$! So we can have a list like
$$
\log(n) << \sqrt{n} << n^{.7} << n << n\log(n) << n^{2.1} << 1.1^n << 2^n << e^n << 20^n.
$$

Feel free to play around with other functions you may find interesting.

---

## More Advanced Stuff

Here I will discuss some more advanced things that specifically computer scientists do with big $O$ notation, that the normal person will not really interact with all that often

### Average Case

When people say things like "oh yeah, this algorithm is $O$ of $n$", often times they think they are referring to the *average* case instead of the worst case, which Big $O$ truly is. Like all of this things, "average" case has a really technical definition[^3].

In the simplest terms, you would take all possible operation counts for all possible inputs (as a function of $n$) and take an average. But since this is in the section entitled **Advanced Stuff** I can fuck with y'all a little bit more who hate math lmfao. You see, before we assumed that all possible inputs were equally likely to appear when we just straight averaged, but what if one of our inputs is one million times less likely than all the others? Well we shouldn't consider that input at the same weight as all the others! We can actually weight our average then by a probability distribution of our input space.

> **Definition**: Let $A$ be the set of all inputs to a particular piece of code, and $T:A\rightarrow\mathbb{Z}_+$ be an operations function that returns the number of operations for any $x\in A$. Consider the set of all inputs of size $n$ to be $A_n$, with elements of $A_n$ selected at random by probability distribution $p(x)$. Then the **average case** of an input of size $n$ is given as
$$
\mathbb{E}\left[T\right](n) = \sum_{x\in A_n} p(x)T(x)
$$

Remember from probability class that $\mathbb{E}$ represents *expected value*, and we're actually calculating it here the same way we always calculate it.

Actually getting the average case can be pretty challenging, but I might as well provide you with an example. In our previous page we showed that Bubble Sort best case (using our implementation) performed
$$
B(n) = \frac{7}{2}n^2 + \frac{9}{2}n + 6
$$
operations. To perform a singular swap, we would then add $9$ operations to perform, so in order to find the average case of our bubble sort, we need to know what the expected number of swaps for a random array is. For our case, assume any particular array configuration has an equal chance of appearing. It turns out that the expected number of swaps per calculations shown [here](https://math.stackexchange.com/questions/245018/bubble-sorting-question?noredirect=1&lq=1) are $\frac{n(n-1)}{4}$. Multiplying by $9$ and adding to our equation we can find our average case is
$$
\mathbb{E}[T](n) = 3.5 n^2 + 4.5n + 6 + 9\cdot\frac{n(n-1)}{4} = 5.75n^2+2.25n+6.
$$

### Non-Ammortized Time

Consider the line of code `set.add(x)`, what is the big $O$ of this piece of code? From an algorithm cheat sheet, you would be told that insertion into a set is an $O(1)$ operation. However if you start learning more you'd find that in the event of a hash collision then set insertion becomes an $O(n)$ operation. Uh wait what, wasn't $O$ supposed to tell us the worst case in the first place?

When we say that set insertion is $O(n)$, we are working with what is called *ammortized* time, and that's normally what most engineers refer to by big $O$. The idea of ammortized time is that you can average the entire function runtime to get a more realistic idea of the runtime. This is **not** the same as the average case, as average case deals with probability distributions.

A good way to think of this would be if I ran a loop $n$ times, and each time only counted as $1$ operation except for one loop iteration that will take $n$ operations. If we don't know when this long loop will happen, then we might be inclined to say our worst case is $O(n^2)$ as any possible loop could be the $n$ one, however thats not realistic as that case will literally never happen. So instead of thinking every loop will run in $n$ time, we we can instead think about is that this is the same as every loop running $2$ operations, making it actually $O(n)$! 

So when we say that set insertion is $O(1)$, that means that over the course of running your entire code, if you average all the times that you run set insertion, it will look as if it was $O(1)$ time, even though any one particular iteration could be $O(n)$.

### Quasi-Polynomial Time

*Will write this at a later date*

[^1]: No I'm totally not salty.

[^2]: Astute readers would realize that $T(n)$ actually isn't a function cause the same size inputs could have multiple outputs, but we're going to not go too hard with this.

[^3]: In fact there are multiple definitions, but I'm going over the simplest.