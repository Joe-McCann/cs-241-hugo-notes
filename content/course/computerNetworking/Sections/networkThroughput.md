---
title: Network Throughput 🍹
linktitle: Network Throughput 
toc: true
type: book
date: 2025-08-05
draft: false
tags:
    - cs356
    - network fundamentals
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 114
---

## But I'm Not A Gamer?

In the previous section we discussed all about how we can measure different forms of delay that determine our network round trip time, and `ping` speed. However, most users really don't care if data arrives after $50$ms from being sent, or $500$ms from being sent, because most applications don't deal at those speeds.

Much more important to users is the *quantity* of data that is being sent, for purposes such as file transportation or video streaming. We might be able to transmit more data at once, even if it takes longer to reach the destination. 

To understand why, consider the analogy of cars going down a highway. Each car represents a packet of data, and it takes some amount of time to go from $A$ to $B$. If you are on a backcountry, single-lane road then you might be able to travel quite fast, as there will not be all that much traffic. However, not that many cars can fit on a single lane road at once. Comparatively, on a highway traffic might cause each car to take longer, but many, many cars can use the road simultaneously. This is a roundabout way to describe network throughput. 

> **Definition**: *Network throughput*, notated $TP$, is the amount of data sent within a given window of time, often measured in bits/second. *Network bandwidth*, notated $B$, is the maximum throughput of the network.

Now "throughput" is a bit of a loose term, because depending on what application we are measuring, we might be computing it differently. For example, are we considering throughput of the entire network, or just one link? A end user who's video streaming may only care about the overall network performance, but a network admin might want to know where a problem lies.

Do we count packet overhead (additional information used for routing purposes) as part of the "data" or no? Counting the packet overhead will give a clearer picture on performance, but may obscure things for the end user.

When asking you questions I will be clear, because the way we define throughput is subject to change. Don't even get me started about the term bandwidth, which comes from signal processing!

> **Example**: If $100$ Gigabits are transmitted across over a link in the span of $5$ minutes, what is the average throughput per second of that link in that time?

{{% callout info %}}
<details>
<summary>Answer</summary>
Average throughput is just bits over time, so 
$$
TP = \frac{100\cdot 10^9}{5\cdot 60} \approx 333\cdot 10^6
$$
which is $333$ Megabits per second.
</details>
{{% /callout %}}

For nearly all users of modern computer networks, throughput is the most important measure of network performance. Consider most things people use the internet for: video streaming, downloading files, thirsting over people on Insta. All of that requires the transmission of sometimes large quantities of data over your network, and it is *very* noticeable when that data can not be transmitted quickly.

For example, imagine we want to watch a $4$K video at $60$ frames per second. An **uncompressed**[^1] $4$K image is $24$MB according to Google, which means that in one second we need to receive at least
$$
24\cdot 60 = 1440
$$
$1.44$ **GIGABYTES** of data per second. If your network throughput is lower than this, then you will not be able to get new data to keep up with all the data you are streaming, and will thus get stuck in buffer hell.

Also note, that when you pay for some internet speed from your Internet Service Provider (ISP), you are paying for the *bandwidth*, which is not guaranteed, and in fact unlikely, to be the actual throughput that you experience at any point in time. What a fucking racket lol 🎾. 

### Network Bottlenecks

While we will discuss later some things that lead to slow downs inside the network, however there are some things that we can touch on now.

The first is that if you have multiple hosts attempting to use the same link to communicate, those hosts will all **IN THEORY** equally divide the bandwidth between each other. Major emphasis on the "in theory" there, as we will see in the "Link Layer" chapter. 

This means that if you are paying for $100$ Megabits per second for your internet service, but you are watching Youtube on your phone, Netflix on your TV, your sister is browsing TikTok on her phone, your brother is browsing TikTok on his phone, and your parents are watching Netflix on a TV, then that's $5$ connections to your access network, and thus each of your devices are **IN THEORY** going to get at max $20$ Megabits per second of throughput.

This is not unique to your access link by the way, you are also sharing links throughout the entire network. In fact, this will limit your performance you see, because the speed that you observe will actually be the *slowest* of all speeds you experience across the entire network! 

Consider the following scenario that refers to the poorly drawn diagram shown below. 

> **Example**: Four people want to download the latest update to Valorant from a server. This update is $1$ Giga**byte**, and all links show their speeds. What average throughput will each user experience and how long will it take to get the update?
> {{< figure library="true" src="computerNetworking/page4_networkbottlenecks.png" title="Server at bottom with Valorant update" lightbox="true" >}}

While we might think that each individual will send data at whatever their access link speed is, that is not the case. This is because even if person $A$ wants to receive their update at $1$ Gbps that their link can handle, the link connected to the server can only send $10$ Mbps of data at a time! 🐢 Thats like hoping to go $100$mph on the highway when you're trapped behind a grandma driving: you're stuck. 

Not only that, but $4$ people are actually sharing that link which means that **IN THEORY** each person would experience $2.5$Mbps of data, and thus would complete their update at
$$
\frac{8\cdot 10^9}{2.5\cdot 10^6} = 3200.
$$

So $3200$ seconds or about $54$ minutes. You can imagine that the person who was paying for $1$ Gbps who thought their download would be completed in $8$ seconds must be super salty lmfao. This is called a network bottleneck.

> **Definition**: A single **bottleneck** is a point that restricts the flow of data and slows down the entirety of the network as a consequence.

In our example, the server could not send data fast enough, and became a bottleneck. However, the same problem would occur if the hosts were trying to send too much data than the network could handle; packets would overload the network and get dropped by routers that were completely filled up. A good follow up question is "how would the host know what rate to send their data at if they only know their own link speed?", which will be answered later down the line. 

A simple reason that bottlenecks can occur as we saw above is due to some links being slower than others, but that's not always the case. Sometimes router processing time may be the problem, other times some links might have an insane amount of traffic. We will discuss some of this in the future.

[^1]: Nobody streams shit uncompressed