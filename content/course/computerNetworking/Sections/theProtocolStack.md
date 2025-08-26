---
title: The Protocol Stack
linktitle: The Protocol Stack
toc: true
type: book
date: 2025-07-14
draft: false
tags:
    - cs356
    - network fundementals
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 112
---

## How Do We Communicate? 

In the previous section we detailed how a computer network can be looked at from the topological perspective, analyzing all the different components and how they connect together. However, very often from the perspective of a computer scientist, the actual details of what is connected to what are not really all that important. 

In fact, it is just as effective to consider the links of the network to form some random graph that other people like Network Administrators need to concern themselves with, while we deal with actually programming these devices to get them to properly communicate with each other. As an analogy, this is like concerning ourselves with the languages are mannerisms that people actually use to talk, rather than the specifics of who talks to who. These "mannerisms" and rules of communication will be the *protocols* of our network.

> **Definition**: A *protocol* is a set of rules for formatting and processing data for the purpose of communication. [^1]

To see an example of a protocol in action, consider what happens when you connect your phone to Wi-Fi in a classroom. Your phone is sending messages back and forth between a wireless access point. But if your message is long, how do you split it up? If your message gets corrupted along the way, how do you recover? Is the data sent encrypted or not? All of these things are decided beforehand dependent on the selected protocol, which in the case of Wi-Fi is `IEEE 802.11`. 

For networking purposes, new protocols are discussed using *Requests for Comments* "RFC"s[^5]

### The Protocol Stack

Computer Networks are *complicated*, and the problem is that there are so many different aspects that need to be addressed when creating protocols. These aspects could be super high level, such as determining what method of encryption your data will use, or super low level, such as how fast the electrical pulses will be to represent $1$'s and $0$'s. However, with all thats going on, it would be wonderful if we could just worry about whatever area of our network and specific problem we are working on, and then take every other aspect for granted. 

This is where the idea of a protocol stack comes in. In the protocol stack, we have different "layers" that represent different pieces of our computer network functionality. All other layer functionality is *abstracted* away, and we can assume that things will just work without needing to care about the nitty gritty.

For example, if I am writing a website, why should I care if my user is connected to their home network via Wi-Fi or Ethernet? To me that would be stupid, I should be focusing on adding stupid brainrot, not how individuals decide to utilize the network!

The theoretical model of the protocol stack that was designed is called the OSI model[^2]. The designers came up with $7$ different layers that they believed to be the most clear division between how network protocols should be defined. 

However, they took to long, and their model/protocols weren't free, so nobody used them[^3]. Instead people came up with the TCP/IP system which designed two protocols (TCP for transport, and IP for network), and said "I literally don't give a fuck what happens outside of this, you do you". Since then, the ideas of the OSI model have bled and merged with the TCP/IP model, to the point that people still refer to them as "layers", and some "layers" from the OSI model were pulled over. Our book uses the TCP/IP model.

{{< figure library="true" src="computerNetworking/page2_osi_tcp.png" title="Two Different Protocol Stacks" lightbox="true" >}}

For the TCP/IP model, the layers can be summarized as follows

1. Physical Layer: Physical mediums that are responsible for how the data physically travels between devices.
2. Link Layer: Layer that describes protocols regarding how devices move from one node in the network to the next
3. Network Layer: Layer that describes protocols regarding movement between artibtrary nodes in the network
4. Transport Layer: Layer that describes protocols regarding how data is transmitted from one process on a host to another
5. Application Layer: Layer that contains all applications that utilize the network

If you would like a description of the OSI Model's Presentation and Session layers, be sure to check out the reference links provided, however for out purposes all of that is contained within the Application Layer.

Now note, that this is a bit of wishful thinking. It turns out that with all abstractions, they evetually leak into each other[^4] and not all designs fall so neatly into them. As we progress through class, we will see many instances where the various layers blend in leak in ways that break the fundementals.

[^1]: https://www.cloudflare.com/learning/network-layer/what-is-a-protocol/
[^2]: https://en.wikipedia.org/wiki/OSI_model
[^3]: https://networkengineering.stackexchange.com/a/6381/101399
[^4]: https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/
[^5]: https://www.rfc-editor.org/