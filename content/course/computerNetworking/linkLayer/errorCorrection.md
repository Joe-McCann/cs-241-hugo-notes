---
title: Error Detection Methods 🤮
linktitle: Error Detection
toc: true
type: book
date: 2025-09-18
draft: false
tags:
    - cs356
    - link layer
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 204
---

## Error Detection and Error Correction

With all the discussion on the previous page, there is a follow up question of how a receiver of a message might realize that something has gone wrong, and thus act accordingly. The last thing we would want is for the message containing the text "whatever you do, don't launch the nukes!" to get garbled up and passed along anyway without providing the possibility for correction. By "error" in a message, we are referring to an idea that the receiver has received a bit when the sender actually had sent the opposite: through some cosmic fuckery, we have a $0$ to $1$ or a $1$ to $0$. 

> **Definition**: An algorithm that determines whether an error has occurred is an **Error Detection** Algorithm, and if it can fix a found error, then it is an **Error Correction** algorithm.

In this page we will really only concern ourselves with detection algorithms. Also, error detection algorithms are used in literally every layer, so why are we talking about them here in the link layer? 

1. I needed another topic for the link layer
2. The link layer uses hardware to efficiently implement more complex algorithms
3. If you can detect errors at every hop, then there is no need to worry about errors later down the line.

Keeping this in mind though that the idea of some of these algorithms will appear later in the future. Technically, all of the algorithms I will show actually fall into the same category of algorithm, in that they are all "checksums", however, I will not show how we can define them all in that way (as that might be confusing).

However, in *all* our algorithms, these will work by taking some binary data $D$, appending on some error detection binary string $C$, and sending that all as a packet $P=D\circ C$, if $\circ$ represents concatenation.[^1]

For this page, knowledge of modular arithmetic will be very helpful, so I'd suggest checking out my mod math page. Also, we will define some notation here that will be very helpful for these sections. Suppose I have some packet $P=D\circ C$ where $P$ is $n$ bits long. We will say that after transmission that the receiver receives $P'$, which is $P$ with potentially some errors, and the **error string** $\varepsilon$ of $P'$ is an $n$ bit string that contains a $1$ in every location there is an error. 

So if $P=1011$ and the receiver gets $P'=0111$, then there were errors in the first two bits. Therefore $\varepsilon=1100$.

----

### Parity Bits 0️⃣1️⃣

The most straightforward error detection technique is to always send a packet that has an even amount of $1$'s, and if the receiver determines on the other side that there is an odd number of $1$'s, then there must be an error. How does this work if we want to send an odd number of $1$'s in our message for real?

We utilize what is called a **parity bit** as our error detection string $C$, yes note this "string" is literally one bit. We append the parity bit to the end of our data $D$ such that our packet $P=D\circ C$ has an even number of $1$'s. As such, if $D$ has an **odd** number of $1$'s, then $C=1$ to make it even. Otherwise $C=0$ because there is already an even number of $1$'s.

From a mathematical perspective, if the digits of $D=d_0d_1d_2\ldots d_n$, then
$$
C = \left[d_1+d_2+\ldots + d_n\right] \mod 2 = \left[\sum_{i=1}^n d_i\right] \mod 2
$$

Remember that "mod 2" just means "take the remainder when divided by 2". Using modular arithmetic, the parity bit algorithm is really easy.

1. When sending data $D$, calculate parity bit $C=\left[d_1+d_2+\ldots + d_n\right] \mod 2$
2. Create packet by appending $P=D\circ C$, and send
3. When receiving $P'$, the receiver computes $\left[p_1+p_2+\ldots + p_n + p_{n+1}\right] \mod 2$[^2]
    1. If result of computation is $0$, then no error
    2. If result of computation is $1$, then flag as error and discard

For example, say our message is $D=11011$. Since there are an even number of $1$s, then $C=0$ and $P=110110$. If the receiver gets the message $P=111110$, which means $\varepsilon=001000$, then we see that there is an odd number of $1$s, and detect the error.

How effective is this method? For us to flag $P'$ as having an error, we need for there to be an odd number of $1$s, which requires an odd number of bits to have been flipped. Should an *even* number of bits flip, then the detection method would not realize than an error has occurred. For example, if in our previous example of $P=110110$, should $P'=111111$, then we would see an even amount of $1$s and thus mark no error. We can formalize this as

> **Theorem**: Parity bit checks will detect an error if and only if $\varepsilon$ has an odd amount of $1$s. 

This can be proven in a straightforward manner with the following heuristic: let `count` be the amount of bits in $P$ that are $1$, we know this is even. Every time there is an error, one bit flips, which either increases or decreases the count by $1$, to an odd number. If this occurs again, then `count` will go back to even. Repeating this process shows that only odd amount of errors will be detected.

> **Corollary**: Parity bit will detect $50\\%$ of all possible $\varepsilon$ strings

This is straightforward, as half of all possible error strings have odd amounts of $1$s. Note, not all error strings are equally likely to occur, but still we should want to find a method that is much better at detection than this.

### $n$-bit Checksums

We will now discuss a generalized version of the parity bit called an $n$-bit checksum. We say this is generalized, because our parity bit algorithm is actually just a $1$-bit checksum. Given our data $D$, we are going to split our data into chunks of size $n$ (padding with zeros on the left as needed) and then add them all up ignoring overflows. This is equivalent to adding all of the values $\mod 2^n$. Finally, our checksum $C$ is the number such that when added to the previous sum, ignoring overflows, we get zero. If our $n$ bit chunks of $D$ are notated 
$$
    D = d_1\circ d_2 \circ \ldots \circ d_k
$$
remembering that each chunk is $n$ bits, then we need to find a value of $C$ such that
$$
    C + \sum_{i=1}^k d_i = 0 \mod 2^n \implies C=-\sum_{i=1} d_i \mod 2^n
$$
you can get that last number by subtracting $2^n$ by the number you get when adding all the chunks without overflow.

Appending the data to the checksum, we send the packet $P=D\circ C$. The receiver then divides the packet into chunks, computes the sum without overflow, and if the sum is not $0$, then states there was an error. So the algorithm is

1. Split data into $n$ bit chunks, and add them all without remainder, take $2^n$ minus that sum to be $C$
2. Send $P=D\circ C$
3. Receiver splits packet into $n$ bit chunks and adds them without remainder
    1. If $0$, then there was no error
    2. If not $0$, determine error and discard the packet

Lets provide an example, suppose we want to do a $3$ bit checksum on $01110101$. We will split this into $3$ bit chunks to add
$$
    01 + 110 + 101 = 1100 \rightarrow 100
$$
notice how we converted from $1100$ to $100$, since we are doing a $3$ bit checksum, we only care about the last $3$ bits (this is what I mean by "without overflow") any overflow bits over the $3$ bit limit are discarded. As such our intermediate sum is $4$. 

Our checksum will be whatever value $C$ converts sums that to $0$, or using modular arithmetic, $2^3-4=4$ so $C=100$. Appending this to our message we get
$$
    P = 01110101100.
$$
If we were to add all of these together assuming no error when transmitted, we would see that
$$
    01 + 110 + 101 + 100 = 10000 \rightarrow 000
$$
and therefore no error. However if we had $\varepsilon=10100000001$ with 
$$
    P' = 11010101101
$$
we would then see that
$$
    11 + 010 + 101 + 101 = 1111 \rightarrow 111
$$
and thus would discard it for having an error.

It can be proven that $n$ bit checksums will miss $\frac{1}{2^n}$ errors, which is pretty good! However, we still want to do better, but the final method is evil.

## Cyclic Redundancy Checks (CRCs) : The Most Despised Algorithm by Students

This next method is so dastardly, that I am first going to describe the algorithm before giving the mathematics that explains why we are doing what we are doing. This algorithm is built around the `XOR` operation. Remember that `1 XOR 1` is $0$.

The algorithm for an $n$ bit CRC goes as follows. 

1. Before a message is sent, both the receiver and sender agree on a $n$ bit generator. This generator will be often decided on a per-protocol basis so that it is initialized on startup.

2. The sender takes its data $D$ and appends $n$ $0$s to the end of the message, which we will denote as $D'$

3. The sender performs long division using `XOR` (emphasis, instead of subtraction we use `XOR`) using $1\circ G$ ($G$ with a $1$ appended to the front) on $D'$, and stores the remainder $R$ (which should be $n$ bits) in the last $n$ digits of $D'$ that were previously our appended $0$s. $D'$ now becomes the packet that we send $P$

4. The receiver gets the transmitted packet $P'$ and divides it by $G$ using the same `XOR` division.
    1. If the remainder is $0$, state that there is no error
    2. If the remainder is not $0$, discard as there is an error

uh WHAT? What the hell does `XOR` long division even mean? Lets go through some examples of `XOR` long division first. 

`insert long division here`

So now we know that if we were to divide $10110$ by $111$ we would get a quotient of $111$ with a remainder of $10$.

To be specific, lets say that we have message $10101$ and want to perform a $3$ bit CRC with generator $011$. First, we append $3$ bits to out data to get $10101000$ then divide it by $1011$ to get a remainder of $101$. As such, our final data we send to the receiver is $P=10101101$. When you divide $P$ by $1011$ you will find that there is no remainder. I encourage you to try this out by modifying to by modifying the error and seeing that the error is caught. 

CRCs are widely used because they have a high chance of catching errors, for any specific error the chance of catching it is $\frac{1}{2^n}$ for an $n$ bit CRC, HOWEVER, special CRC generators can be selected to catch errors that have certain forms. For example, no "burst" errors less than $n$ bits long will ever be missed, or all errors that contain less than $6$ flipped bits will be caught. Etc etc. 

### Why does this work? 

Get ready for a deep dive into some hardcore abstract algebra.


[^1]: Technically, these codes are put in the middle of the message, not the end, but thats not important for our purposes.
[^2]: Note its $n+1$ bits because we added the one additional parity bit
