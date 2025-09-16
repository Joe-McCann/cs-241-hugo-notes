---
title: Broadcast Networks 🛜
linktitle: Broadcast Networks
toc: true
type: book
date: 2025-09-13
draft: false
tags:
    - cs356
    - link layer
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 203
---

## What if Nobody is in Control?

In our previous example of switched networks, there were hidden switch devices that were responsible for the forwarding of communication between various devices, and in this way the network works wonderfulling like magic. This is why, according to the book, L2-switched networks are growing vastly in popularity over larger distances than ever before. 

However, not all LANs are switched in this way, the original LANs were a bunch of cables that were all connected together so that if any one host sent a message, *every* host would hear it. 

> **Definition**: A **broadcast network** is one in which every message sent is broadcast to every host on the network.

Note that this does not mean every destination is written as the ff-ff-ff-ff-ff-ff, just that even addressed packets will be heared by unintended recipients. 

Two incredibly popular examples of network protocols that are used for broadcast networks are Wi-Fi and Bluetooth. Prior to them radio waves were also a type of broadcast network (and still are). 

To understand what will make broadcast networks so challenging compared to switched networks, is to think about a group setting in which a bunch of people who don't know each other are sitting in a circle and having a collective conversation. Even if multiple people want to say something, only one person at a time can speak, because otherwise the multiple conflicting voices would drown each other out. 

> **Definition**: A **collision** on a broadcast network is when more than one device attempts to broadcast simultanously, and corrupts the messages. The problem of dealing with potential collisions is the **Multiple Access Problem**

So we understand that if multiple people are talking at the same time, we are going to waste time because nobody will understand the messages. 

What we will discuss here are various **algorithms** (not protocols) for dealing with problems relating to mitigating collisions. Thinking back to our group conversation example, the worst things that can happen are long periods of awkward silence, and one person who is dominating the conversation. As such, we have two standards that we would like to have for our algorithms: Assuming that the max throughput of our link is $R$ b/s, then 

1. If only $1$ device is speaking, it transmits at rate $R$
2. If $n$ devices need to speak, on average they will have throughput of $\frac{R}{n}$

So effectively just make sure all communication is fair and even. Sounds good. With that in mind, lets discuss the most straightforward of algorithms

### Channel Partitioning Algorithms

The most straightforward of algorithms to solve the multiple access problem are algorithms that take the transmission of data and divde it between the devices on the network. These types of algorithms are called **Channel Partitioning** algorithms because they partition the broadcast channel (obviously). 

The textbook goes over $3$, but I will only discuss the two easiest here: time and frequency partitioning. 

**Time partitioning** is when, assuming we have $n$ devices on the network, we equally divide all the time into slots, of which the devices rotate taking turns broadcasting during that time. Suppose we have $3$ devices, and we split the time into $1$ second time slots, then in the first second device $1$ would broadcast, then device $2$, then device $3$, and then at the start of $t=3$, device $1$ would be able to broadcast again.

This algorithm is great because we have literally $0$ collisions, as if it is not your time slot then you cannot speak. Not only that, but time is divided equally between all devices of $\frac{R}{n}$. However, the problem is that if a device is not speaking, then its time slot will be wasted. As such, no device can possibly do better than $\frac{R}{n}$ when implementing this algorithm. 

**Frequency partitioning** is when the entire frequency spectrum of the network is divided into smaller frequency bands that do not overlap. In this way, every single host can transmit simultaneously without colliding, however if the total bandwidth of the link is $B$, then each device will get $\frac{B}{n}$ bandwidth, and thus transmit at a rate of $\frac{R}{n}$. An example of this is FM radio, in which each of the different radio stations have their own radio bands that you can listen in on at the same time. 

Note that the difference between these two, despite having the same average throughput of $\frac{R}{n}$, is of how that average occurs. In the time version, each device can broadcast at a rate of $R$ for $\frac{1}{n}$ units of time, whereas in the frequency version, each device broadcasts at $\frac{R}{n}$ for the entire time. I encourage you to come up with some cool examples to think about when you might choose time over frequency and vice versa.

### Random Access Methods

In an effort to try to make usage of the channel more efficient when not many hosts need to communicate at any given point in time, we need to have a solution that is more ok with the idea of packet collisions. In a **random access** protocol (protocol that uses a random access algorithm), a host transmits whenever it needs to transmit. However, the host is also listening to the line while its transmitting, and if it realizes there is a collision, then it will wait a random amount of time before attempting to rebroadcast. Consider why it would need to be a random amount of time.

As a case study on random access methods, we will show one of the first random access protocols which is called "Slotted ALOHA", named because it is a slotted version of the ALOHA protocol. Assuming that our link has a transmission rate of $R$ and retransmission probability $p$ then

1. Every packet has $L$ bits
2. Time is divided into slots of $\frac{L}{R}$ seconds, so that transmitting a packet takes exactly one slot.
3. All transmission can only begin at the start of a slot
4. All nodes in the network are synced so that their slots begin at the same times
5. If two or more packets collide, then all nodes detect the collision before the slot ends
6. If a node detects a collision, every subsequent slot it will decide to retransmit the packet with probability $p$ until the packet is successfully transmitted.

How well does this method work? Well if we have only one host that needs to transmit, then we will get the full rate $R$ with no problem. How does it work though if we have multiple devices on the network? 

For an **incredibly simplifed** mathematical model of how Slotted ALOHA performs, we can assume that there are $n$ devices, and any single device on the network will want to transmit on any given slot with probability $p$. As such, when we want to know how efficient our model is, we want to know what is the probability that during any given slot we will have **exactly** one device transmitting.

The probability that one *specific* device is the only one to transmit during a given slot is $p\cdot(1-p)^{n-1}$, because there is a $p$ chance that the device wants to transmit, and a $(1-p)$ chance that any of the other $n-1$ devices do not want to transmit. Since there are $n$ total devices, we add all these together to get
$$
P(\text{successful transmission}) = np(1-p)^{n-1}
$$

Now if you do some calculus, you can prove that at maximum efficiency as you scale the network, your theoretical limit will be that there is a $\frac{1}{e}\approx 37\\%$ chance that any given slot will contain a successful transmission. That could be a good exam question to do that calculus honestly. 

Anyway, this rate sucks, because if you have a $100$ Mb/s link, this means that on average you can expect that you will only have an efficiency of $37$ Mb/s, ouch. 

You might think that ditching the slots would make things better, of which the answer is they can but we need to be more clever. If we were to treat everything the same but remove the slots, then the network efficiency in the limit would drop to $\frac{1}{2e}$[^1]!

### The Incredibly Obvious Idea of Carrier Sensing

When we decide to utilize a broadcasting algorithm that has the potential for collisions, there is are two insanely obvious enchancements that we can add our algorithm to make it more efficient immediately. Remember that a NIC actually listens and sends simultaneously, so if we notice that someone else is sending a message, why don't we just not?

1. Collision Avoidance: If you detect another node on the network broadcasting, wait until they are done before speaking.
2. Collision Detection: If you detect another node broadcasting while you are currently broadcasting, then a collision has occurred and stop broadcasting.

[^1]: Proof in the textbook