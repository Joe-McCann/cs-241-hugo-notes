---
title: Network Performance ⏩
linktitle: Network Performance 
toc: true
type: book
date: 2025-08-02
draft: false
tags:
    - cs356
    - network fundementals
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 113
---

## Why Does My Internet Suck?

The most common shared experience of users of the internet is when the network is moving slowly, and you get the urge to throw your device out a window[^1]. Examples that will cause PTSD in students are many of the following:

1. When you want to watch a movie on Netflix, but it seems to keep buffering every 5 seconds, so in reality you feel like you are watching stop motion video
2. When you are playing League of Legends and your ping spikes during the middle of a teamfight, and now suddently the game freezes, and by the time you reconnect to the game you are dead.
3. When you are watching livestream and suddenly it cuts out during a cool moment, and by the time it comes back you've jumped past the cool moment and the chat is going crazy without you.
4. When you are trying to have a virtual job interview, but your interviewer cannot seem to see or hear you because of your connection issues. 

and so so so many more. In this page we will discuss different measurements for network performance, and how depending on your use case, you might want to optimize for one metric over another. As we continue through the course we will discuss many different reasons that slow downs might occur, so whether your Youtube videos keep buffering, or your Marvel Rivals game keeps lagging, you can know what's up. 

---

## Round Trip Time (RTT)

The simplest measure of network performance is packet Round Trip Time (RTT), which is literally just imagining we start a stopwatch when we send some packet out, and stop it when we get back a response.

> **Definition**: The **round trip time** (also known as **ping time**) from Host $A$ to Host $B$ is the amount of time it takes for Host $A$ to receive a response from Host $B$ after sending data, notated as $RTT(A,B)$ 

RTT is the best friend metric of gamers everywhere as very often "ping" is the most important statistic. This is because most online video games don't send a large amount of data back and forth between you and the server at any moment in time. As such, we don't really care if our network is able to handle giant dumps of data at a time, but instead want to make sure that our data reaches its destination as fast as possible. For some games, a ping time of even $100$ms can feel disasterous. 

> **Example**: If Host $A$ is sending a packet to Host $B$, it takes $20$ms to reach $B$, $5$ms for $B$ to process the packet, and $30$ms for the response from $B$ to reach $A$, what is the Round Trip Time?
<details>
<summary>Answer</summary>
The Round Trip Time is the total time from $A$ to $B$ and back, so in this would be
$$
    RTT(A,B) = 20+5+30 = 55
$$
which makes the answer $55$ms.
</details>

Notice that in the previous example we actually count the processing time required for Host $B$ to provide the response as part of the Round Trip Time. This could be highly variable depending on the application of the packet you are sending. Consider if you were to send a request to ChatGPT, and it takes $50$ms to arrive, $5$s to process, and $45$ms to send back. We would have
$$
RTT(\text{you},\text{ChatGPT})=50+5000+45 = 5095.
$$

In this case $98\%$ of the RTT is actually the ChatGPT processing time! From your perspective you only see the RTT and have no idea whether or not the slow down is related to network performance. 

A good analogy of this would be if you were watching your friend at a marathon in which the start and finish line were at the same location, and you stayed there to time them start and finish. If your friend takes a super long time to finish, you have no idea if they just suck and are a slow runner, or maybe they are a fast runner but just stopped midway through the race to buy and eat a hot dog. 

As such the RTT time is often measured in practice using the `ping` command on terminal, because if Host $A$ sends a `ping` to Host $B$, $B$ will immediately fire back a `ping` response to $A$, which minimizes the processing time and gives a good insight into the underlying network performance. Note however, that *technically* `ping` time and RTT for a process are not the same, as `ping` uses a special type of packet (Internet Control Message Protocol, which we will discuss in the futurue), and may be treated differently for this reason. In practice though, `ping` gives a good enough metric to get an idea of network performance.

### Types of Delay

[^1]: Is this just me?