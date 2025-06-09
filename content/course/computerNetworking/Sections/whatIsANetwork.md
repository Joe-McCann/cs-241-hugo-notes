---
title: What is a Computer Network?
linktitle: What is a Network?
toc: true
type: book
date: 2025-06-8
draft: false
tags:
    - cs356
    - network fundementals
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 111
---

## My dear beloved to who I write... 💌

Before even discussing computer networks, I think it is important to say that at the heart of computer networking lies a problem that humans have been working on to solve for thousands of years[^1]: optimizing communication with each other, sometimes across long distances. In the past, this would be done via message carriers, written letters, carrier pigeons, and the telegraph, and now for this class we discuss how these problems that people have been dealing with for thousands of years have found themselves appearing even with the technological advancements of the computer.

For example, all of the following issues that could have appeared in the past will have a similar situation be discussed during this class!

1. How do we ensure that if our message carrier is captured by an enemy, that our messages and secrets won't be discovered?
2. What happens if the enemy shoots down our carrier pigeons? 
3. How do we handle if multiple people need to use the same telegraph line simultaneously? 
4. How can we ensure that the recipient of our message will know if the letter we send has been damaged along the way? 
5. How can I determine where to send my letter to if I do not know the address of the intended recipient?

I bring this up because while it might be easy to become tunnel visioned on this class as just being applicable to the internet, and find it "useless" if you aren't dealing with low level socket programming on the job, I'd argue that the more you consider this class about the art of communication at different levels, you may find it very useful in other areas (in particular soft interpersonal skills).

### Yeah but what is a Computer Network?

Like any communication network, a computer network is a series of connections that allow different computing devices to communicate with each other. We use the term "computer" loosely, as nowadays many items can be connected to the internet. I encourage you to think of particularly funny examples of things that people now connect to the internet such as

1. Toothbrushes
2. Fridges
3. Toilets
4. Cars
5. [Your answer here]

> **Definition**: An end user device that is attached to a network to serve some functionality is called a **host**

We specify "end user" to represent that these are at the very edge of the network and are not being used for intermediate purposes[^2].

In order to connect our hosts together, we actually need to send our *data* over physical connections, such as Copper cable, Fiber Optic, Radio Waves, etc.

> **Definition**: A connection between two devices on a network is called a **link**.

## Getting the Data from Point A to B

When we send "data" over a computer network, that data is represented in a group of binary digits that have some meaning. Perhaps the first digits are a name, then the next part is some message, and the last part says where the message needs to go. On their own, none of these bits actually mean anything, only together as a whole does the message make sense. As such we need a term to describe one "unit" of a message. 

How will we describe these units though? Consider sending a letter over mail. To me, the most straightforward "unit" would be one letter. However, to the mail carrier they might consider an entire bag of letters to be one unit of measurement since from their perspective they just transport bags of mail between distribution centers. Some other people might consider individual words of the letter to be distinct units. 

I bring this up to explain that different use cases and different users might have different ideas *and* terms for what "one unit" of measurement is considered. *Most often,* one unit of data sent over a network is called a **packet**. For all sections, if I use the term "packet", that will just mean one generic chunk of data sent for our purposes, however, other terms may be used interchangibly depending on the purpose.

### The need for switches

The most obvious way to link all of our hosts together would be to physically stick a wire between all of them that need to talk to each other. For a small amount of hosts this works completely fine, but this unfortunately does not scale. 

For $2$ hosts you need $1$ cable, for $3$ you need $3$, for $4$ you need $6$, and the trend continues with the formula
$$
\text{edge}(K_n)=\frac{n(n-1)}{2}
$$
where $n$ is the number of hosts in our system. Note that the symbol $K_n$ is from graph theory in which this represents the [complete graph](https://mathworld.wolfram.com/CompleteGraph.html) of $n$ vertices: a graph where every vertice is connected to all others. The function "edges" counts how many edges we have between vertices. 

This is bad as we scale $\Theta(n^2)$ as we add more and more hosts into our system. In a world with $1$ million hosts (of which there are many more in the modern world) we would need around $10^{12}$ links between devices! This is not feasible and thus we need another way. 

The (hopefully obvious) solution is to use intermediate devices in our network that are able to take in incoming data, and route it off in the correct direction to its destination. Think of this like the network of roads that connect houses together: every house does not have a private road to every other house, rather there are many shared roads (links) that then connect to intersections (switches) where you can adjust you path to get to your destination.

[^1]: Ever since there have been two humans, there has been a need to comunicate and make said communication work. 
[^2]: This line can get blurry with something like your smart phone with a mobile hotspot, which can act as both a host and intermediate switch. As such we will clarify when something is acting like a host or switch if it's ambiguous. 