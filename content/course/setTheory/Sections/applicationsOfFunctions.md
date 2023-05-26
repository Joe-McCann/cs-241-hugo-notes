---
title: Applications of Functions in CS
linktitle: Applications of Functions in CS
toc: true
type: book
date: 2023-05-20
draft: false
tags:
    - cs241
    - functions
    - applications

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 240
---

## Data Structures

We have already talked about how functions in computer science, and how they are also just as functions are in mathematics, but there are many more applications of functions than just this. We will first begin by looking at some common data structures and showing how we can model them using functions.

### Arrays and Maps

When I talk about arrays (or lists for all my `python` homies), you should think of the most simple data structure around. That being an ordered list of items, where each "index" corresponds to a sequential location in memory that houses some item. If I had an array 
```python
arr=["hi","my","name","is","Joe"]
```
then if I selected `arr[0]` I would get `"hi"`, `arr[3]="is"`, so on and so forth. We are inputting a natural into our array index, and getting out the item in that associated location, which almost appears like a function mapping integers to items! Written out we have
$$
\text{arr}: \\{0,1,2,3,4\\}\rightarrow \mathbb{S}
$$
if we let $\mathbb{S}$ be the set of all possible strings. Clearly arrays must be a function, as every index has only one associated item, and picking an index outside your domain will throw an `Out of Bounds` error. If you want to get even more technical, you can consider this the composition of two functions, where one converts the index into a memory location, and the second converts a memory location into an item!  

Lets represent the function `mem` to be the one in which you input an index and get a memory value, and `item` to be the function where you look up a memory value and get an item. Then we would have
$$
\begin{align}
\text{mem}&: I\rightarrow \mathbb{N} \\\\
\text{item}& : \mathbb{N} \rightarrow S.
\end{align}
$$
In this case we let $I$ be our set of indexes, the natural numbers represent memory addresses, and items inside memory are set $S$. From here we can see that our `arr` function from before is just
$$
\begin{align}
\text{arr}&=\text{item}\circ\text{mem} \implies \\\\
\text{arr}(i) &= \text{item}\left(\text{mem}(i)\right).
\end{align}
$$

In the event we don't want to use arrays, but rather use *maps* (or dictionaries), then we can apply the same principle with a slight modification. Instead of our function `mem` taking in an index and converting it to a memory address, we will instead expand our input set to include other "hashable" types instead of just integers. 


For example, assume we want to use strings as our map keys.
```python
dictionary["one"] = 1
dictionary["two"] = 2
```

Here given the string `"one"` we need to know what memory location to look at, so we need a function that converts strings into integers that go from $0$ to $n$, where $n$ is the number of memory slots we have. Sadly, we live in finiteland of finite memory, and there are many many many possible strings. In fact, using only lowercase english letters, there are $26^{10}\approx 1.4\cdot 10^{14}$ different $10$ character long strings. If we gave even just $1$ bit to all these, we would need $17$ Terabytes[^1] of storage space.

Clearly, we need to find a way to compress the set of all possible strings into our limited memory space. In this case `mem` is going to be a special type of function called a **hash** function.

> **Definition**: A **hash function** $h$ is a function that takes in an input from a large input set, and converts it to a value from a smaller output set. In math terms, $h:A\rightarrow B$ where $|A|>|B|$.

Something we will find later is that computer scientists are a weeee bit loosey goosey with their terminology, so when looking up **hash** functions, its referenced usually in terms of its application without really going into the theoretical weeds. For us, we can let $A$ be the set of all "hashable" objects that we want to potentially use as indeces. We can say that our `dictionary` is a function that is mapping input items $i\in A$ into items we store.

$$
\begin{align}
\text{dictionary}&=\text{item}\circ h \implies \\\\
\text{dictionary}(i) &= \text{item}\left(h(i)\right).
\end{align}
$$

---

### Hashing Algorithms

Now lets consider some things that we might be looking for when thinking about a function to pick to use for a hashing function. Suppose we have our input space $A$ and our memory addresses to map to as $M$, with $|A|>|M|$ and we want to find
$$
h:A\rightarrow M.
$$
Pretty clearly we can immediately see from our properties of functions that $h$ cannot be injective, as there are more items in $A$ than $M$. As such it is expected that we have a **collision** at some point

> **Definition**: Given a hash function $h:A\rightarrow M$, a **collision** is a pair of inputs $x,y$ such that $x\neq y$ but $h(x)=h(y)$.

These are two items that would be mapped to the same memory address which would get really confusing. If I inputted $x$ or $y$ into my function, how would I know which item I would want to retrieve from the memory address??

While there are practical ways to combat collisions that we will not discuss here (such as making each memory location a linked list), we can see easily that we want a function that minimizes collisions. To find a function like this, we would preferbly find a function that distributes our input space evenly between all of our memory addresses, so that our odds of getting nice evenly used memory is high.

In a similar way, we would want to make sure we aren't wasting any memory, as you paid good money for that RGB[^2] RAM and need to get good use out of it. As such we want $h$ to be *surjective* so all addresses are hit.

Practically, we also want a function that doesn't take a ton of time to compute, as if time was infinite and not a problem, we would just use an unsorted list of key-value pairs for everything, and we also want our function to be easily modifiable for different co-domains, so if we have more or less memory we do not need a different algorithm.

One pretty easy hash function that converts any integer into a finite co-domain of intergers is the `mod` function, which returns the remaineder of $\frac{a}{b}$. The remaineder is guaranteed to be an integer between $0$ and $b-1$ (inclusive), so we can say
$$
\begin{align}
h&:\mathbb{N}\rightarrow \\{0,1,\ldots,m-1\\} \\\\
h(x)&= x \mod{m}.
\end{align}
$$
Of course this assumes our memory addresses are in a sequential chunk, and that we are only given an integer. We will discuss `mod` quite a bit later, but we could code this by using

```python
h = lambda x: x % 10    
print(h(500))           # outputs 0 as 500/10 has no remainder
```

#### Bitcoin Mining

Probably the most commonly found place for `hashing` in pop science is for the purposes of bitcoin mining. A discussion on what cryptocurrencies are used for and how they work[^3] is a discussion for another day, but the process of `mining` makes use of a special type of hashing algorithm called **Secure Hashing Algorithm 2**[^4] (aka SHA2), specifically $\text{SHA}256$. $\text{SHA}256$ takes in a bitstring of data, and converts said bytes into a $256$ bit long output.

If we let $B=\\{x | x\in\mathbb{N}, x<2^{256}\\}$, then we can define
$$
\text{SHA}256: \mathbb{N}\rightarrow B.
$$
In this case, SHA was not designed to provide a hash for memory location, but rather to be a function that is so ridiculously convoluted, that if I am given some value of $y\in B$ and asked to find a value $x\in\mathbb{N}$ such that $y=\text{SHA}256(x)$, then the only feasible way to do it is via trial and error. This was for encryption reasons initially, but has since been coopted by crypto lmao.

Specifically, bitcoin mining asks the following: given some bitstring $Y$ (that is determined by the blockchain algorithm), can you find some number $X$ that when concatonated to the bitstring to make $x=YX$ we find that
$$
\text{SHA}256(\text{SHA}256(x)) < T
$$
where $T$ is some predetermined value. People also phrase this as "find an output that begins with $k$ zeroes"

*Perhaps in my spare time I'll explain the SHA algorithm, even though it seems to just be a "lmao who knows whats going on" sort of thing* 

[^1]: By "Terabyte" we mean $10^9$ bytes (where one $1$ byte is $8$ bits). Thank you garbage ass metric system for giving us this instead of $2^{40}$ 🤮.

[^2]: Please raise a `git` issue when this joke is no longer relevant and makes me look like a Boomer.

[^3]: [Download page](https://bitcoin.org/en/bitcoin-paper) for the original blockchain paper

[^4]: [Wikipedia](https://en.wikipedia.org/wiki/SHA-2) with link to original paper