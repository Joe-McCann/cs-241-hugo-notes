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
{{% callout info %}}
<details>
<summary>Answer</summary>
The Round Trip Time is the total time from $A$ to $B$ and back, so in this would be
$$
    RTT(A,B) = 20+5+30 = 55
$$
which makes the answer $55$ms.
</details>
{{% /callout %}}

Notice that in the previous example we actually count the processing time required for Host $B$ to provide the response as part of the Round Trip Time. This could be highly variable depending on the application of the packet you are sending. Consider if you were to send a request to ChatGPT, and it takes $50$ms to arrive, $5$s to process, and $45$ms to send back. We would have
$$
RTT(\text{you},\text{ChatGPT})=50+5000+45 = 5095.
$$

In this case $98\\%$ of the RTT is actually the ChatGPT processing time! From your perspective you only see the RTT and have no idea whether or not the slow down is related to network performance. 

A good analogy of this would be if you were watching your friend at a marathon in which the start and finish line were at the same location, and you stayed there to time them start and finish. If your friend takes a super long time to finish, you have no idea if they just suck and are a slow runner, or maybe they are a fast runner but just stopped midway through the race to buy and eat a hot dog. 

As such the RTT time is often measured in practice using the `ping` command on terminal, because if Host $A$ sends a `ping` to Host $B$, $B$ will immediately fire back a `ping` response to $A$, which minimizes the processing time and gives a good insight into the underlying network performance. Note however, that *technically* `ping` time and RTT for a process are not the same, as `ping` uses a special type of packet (Internet Control Message Protocol, which we will discuss in the futurue), and may be treated differently for this reason. In practice though, `ping` gives a good enough metric to get an idea of network performance.

So we know that there is delay in moving from point $A$ to point $B$, but where does this delay come from? There are $4$ main types of delay that cause problems that we will discuss, that have varying degrees of difficulty to calculate or estimate.

### Propogation Delay

The most fundemental type of delay is probably the most straightforward to understand in that any time we send data, the data actually needs to *physically* travel to the destination. 

> **Definition**: The *propogation delay* of a packet sent along a network, is the amount of time the data spends traveling along links, notated as $d_{\text{prop}}$

This speed at which a message travels is heavily reliant on how you are sending it; for example, if you sent a piece of paper mail via USPS, it would probably arrive faster than via horseback. In the same way, Wi-Fi, Copper Cables, Fiber Optic, etc. all send their information at different speeds. This is (apparently) represented in terms of the percent at which data travels at the speed of light called the "velocity factor"[^2]. 

Fiber Optic cable, for example, transmits its data at $\frac{2}{3}$ the speed of light. However, for our purposes we know that nothing can travel faster than the speed of light, so this will provide a fine enough lower bound.

> **Example**: Originally, the League of Legends servers were hosted in Los Angeles. If I played from Newark NJ, provide a lower bound of the propogation delay I could experience (ie: no matter how good my connection is, assuming there is literally no other source of delay, what can my `ping` time never be better than)
{{% callout info %}}
<details>
<summary>Answer</summary>
According to Google, Newark is a little less than $4000$km from Newark NJ. The speed of light is around $3\cdot 10^8$m/s, which means that 
$$
d_{\text{prop}} \geq \frac{4\cdot 10^9}{3\cdot 10^8} = \frac{40}{3} > 13.
$$
However, this is the amount of time it would take a signal to go one way, so in order to return it would double our time, meaning that we know
$$
d_{\text{prop}} \geq 26.
$$
Our ping can never be less than $26$ms. 
</details>
{{% /callout %}}

Of course, this example made a ton of assumptions. First that our signal was traveling at the speed of light, which it will not. If we assume the speed of fiber optic links at $\frac{2}{3}$ the speed of light, our best possible scenario becomes $40$ms.

We also assumed that our data was moving in a straight line directly from Newark to Los Angeles and back. In fact, we have no idea the path that we are going to take, and it might actually be way far off. For our purposes though, we can just use these assumptions to make the math easier, but just remember that things may be more complicated!

### Switch Processing Delay

As discussed before, processing time is considered part of the $RTT$, and as such we use `ping` to try to make it as small as possible. However, Host $B$ is not the only device that performs processing, remember that the path that moves along the network from your device Host $A$ to Host $B$ contains many, many, packet switches (routers, link switches, etc). 

> **Definition**: The *processing delay* of a network switch $R$ is the amount of time required for the router to determine where the next place a packet of data should be sent to is, notated as $d_{\text{proc},R}$.

The total amount of processing delay in your $RTT$ is actually going to be a sum of all the processing times of all the routers along your trip, or more simply
$$
d_{\text{proc}} = n\cdot d_{\text{proc,avg}}
$$
if $d_{\text{proc,avg}}$ represents the average processing time of routers in your path. 

Traditionally, most people (including the textbook) have considered processing time to be fairly insignificant in terms of delay, as routers in the past have processed packets very fast, on the scale of microseconds. However recently switches have been becoming more and more complex and have been required to perform more and more computations. This has led to an increase in processing time as a source of delay[^3] (Ramaswamy, Weng, Wolf 2004).

Since we have no way of knowing what type of router we will be encountering, we will just need to estimate. From the paper cited above, it seems that modern ranges of processing delay come in around $.1\leq d_{\text{proc,avg}} \leq 1$ms, so you can choose worst or best case scenario based off that. 

> **Example**: What would be the processing delay of a message sent one-way from $A$ to $B$ if the average network processing delay is $.4$ms and it you pass through $10$ routers to reach $B$?
{{% callout info %}}
<details>
<summary>Answer</summary>
Multiply the number of routers times the average delay
$$
d_{\text{proc}} = .4 \cdot 10 = 4
$$
Total delay is $4$ms.
</details>
{{% /callout %}}

### Transmission Delay

So far, we have discussed the idea of data traveling along links, and processing time within switches. An astute observer may have noticed that links are not switches and it actually takes some time to actually unload the data from the switches (or even hosts) onto the links. The actually transfer time is dependent on the speed of the link and represents how much data can transmit at a given time, and is distinct from the processing time. 

> **Definition**: The *transmission delay* is the amount of time that it takes to push data onto a link from a host or switch $R$, represented as $d_{\text{trans},R}$

Of all the different delays, this one is actually fairly straightforward to compute, as each switch normally will advertise their transmission speeds. Similar to processing delay, the total across the entire round trip would be the sum of all transmission delays, however for an individual device we can calculate it very easily.

> **Example**: A router $R$ is able to transmit bits onto a link at a speed of $100\cdot 10^6$ bits per second (Mbps). How long would it to transmit Call of Duty Warzone, which is about $100\cdot 10^9$ **bytes** (GB). 
{{% callout info %}}
<details>
<summary>Answer</summary>
Remember that $1$ byte is $8$ bits, so to know the total time it would be
$$
d_{\text{trans},R} = \frac{8\cdot 100\cdot 10^9}{100\cdot 10^6} = 8\cdot 10^3
$$
This means it would take $8000$s or about $2$ hours and $13$ minutes to *transmit* the entire file.
</details>
{{% /callout %}}

Note that when you are paying for "internet speed of $1$Gbps", you are paying for a link that accepts this amount of data *at most*. It is entirely likely that most of the time you will be transmitting at a slower rate. Also note (and we will discuss this in the next section), that just because you can send data out at a super fast rate, does not mean the network can handle your data at that same rate... 👀

### Queueing Delay

The final point of delay is so complicated, and so variable, that an entire branch of mathematics[^4] has been dedicated to it. It also helps that it is useful for things other than just networking, because no matter what you are doing, someone is going to have to wait in a fucking line 🤮. The last type of delay is literally how long do you spend waiting for other people (or devices) to do their shit before I can do my part. 

> **Definition**: *Queueing delay* is the delay that is caused by packets waiting in a queue to be processed or transmitted, notated as $d_{\text{queue}}$.

As far as I am aware, I am not going to ask computational questions regarding queueing delay, because the amount of time you wait in a line is dependent on how much traffic is on the network at any given point in time. Also note that in switches there are **two** places you may need to wait in line: before processing or before transmitting. 

This delay should make sense, think about having to wait in line for Taylor Swift tickets (impossible) vs. Black Eyed Peas (without Fergie), one is going to be a longer process than the other. Perhaps if I decide to torture future students I will go over some Queueing theory, but for now, we will just leave it as is.

---

### An Analogy for Delay Types

With all these different forms of delay, it can sometimes become easy to get confused regarding which is which. As such I find it to be a good excercise to go through the effort of describing in terms of analogies different ways to think of these network delays. I will provide one, but I suggest going through the effort of coming up with one of your own as well.

Imagine we have someone taking NJ Transit at NY Penn Station[^5]. In order to catch their train, they arrive at the train station and proceed to figure out what track they need to go to (*processing delay*). They then need to take an escalator down to the track, however there are so many people that they need to wait in line to get on the escalator (*queueing delay*). After waiting in line they take the escalator down (*transmission delay*), and then get on the train which takes them to their destination (*propogation delay*).

[^1]: Is this just me?
[^2]: https://en.wikipedia.org/wiki/Velocity_factor
[^3]: http://www.ecs.umass.edu/ece/wolf/pubs/globecom2004.pdf : Ramaswamy, R., Weng, N., & Wolf, T. (2004, November). Characterizing network processing delay. In IEEE Global Telecommunications Conference, 2004. GLOBECOM'04. (Vol. 3, pp. 1629-1634). IEEE.
[^4]: https://en.wikipedia.org/wiki/Queueing_theory
[^5]: For anyone who is not familiar with this process, let me tell you that it is a horrible, agonizing process.