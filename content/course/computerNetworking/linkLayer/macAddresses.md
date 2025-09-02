---
title: Media Access Control Addresses
linktitle: MAC Addresses
toc: true
type: book
date: 2025-08-26
draft: false
tags:
    - cs356
    - link layer
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 201
---

## Who Are You 🎵?

Imagine that Host $A$ and Host $B$ are in the same room and want to communicate with each other, and lets assume for now that communication is totally possible across some medium (like Wi-Fi) without any problems. These two devices are so close that **IN THEORY** they should not need to go through the entire internet just to end up back in the same location. We should be able to "directly" send messages while over the small network that we have locally before entering into the router. This idea of a small local network leads to the popular definition

> **Definition**: A **local area network (LAN)** is a network of devices that communicates over Link-Layer (L2) protocols[^1].

To add some color to this page, when I was in high school, everyone had downloaded a version of Halo $1$ on their laptops[^2] and we even installed it on the school computers in the computer lab. One night during a school event, we had a gigantic LAN game in which $16$ or so people all played together. Note this game was old and did not have any active servers, so in order to play, all of our devices were communicating with each other without going through the router to get onto the internet[^3].

This is the idea of a LAN, we have a little mini network in which we don't need to worry about routers and finding the path through the internet, because everyone is directly connected **IN THEORY**. Since we don't need to pop on into the routers for path finding purposes, LANs are considered Link-Layer (L2) networks. As of right now, we will not worry about whether our L2 network uses Ethernet, Wi-Fi, or Bluetooth, but we will discuss that later in the future.

In this chapter, we will be discussing some of the protocols that are used in order to navigate around on LANs, so that should we in the future want to exit our LAN onto the wider internet, we can do so. 

### LAN Addresses 

If $A$ wants to send a message to $B$, how do we specify who $B$ is? Just like sending a text message to your crush, you have to provide some digits (address). For a phone this is a phone number, but for a LAN this is what is called a MAC Address. 

> **Definition**: A **Media Access Control Address (MAC)** is a $48$-bit network address used for the purpose of communication within a LAN. MAC addresses are typically notated in two-character hexadecimal strings: a6-01-b3-fd-23-cc for example.

MAC addresses **IN THEORY** are supposed to be globally unique, and thus in practice if you have two devices with the same MAC address on a LAN, the network is going to crash out. Since there are $2^{48}$ possible MAC addresses, this is a safe bet. 

A special MAC address that we will use soon is called the *broadcast* MAC Address and is just all $1$s ff-ff-ff-ff-ff-ff. A message with the broadcast address will be sent and received by every device on the LAN. 

Your laptop most likely has many different ways that it can connect to a network; three immediate options are Wi-Fi, Ethernet, and Bluetooth. Does your CPU need to understand all these different protocols? What if there is a new type of protocol that's super fast, that would require throwing out your entire CPU for literally such a minor problem. As such, these Link-Layer protocols are actually handled by dedicated chips called NICs.

> **Definition**: A **network interfact controller (NIC)** is a physical chip that connects a host to a L2 protocol, such as Ethernet. Every NIC has its own unique MAC address.

Why would each NIC have its own MAC address? It is possible to connect the same host to the same network over different types of network protocols, as such if they shared a MAC address its possible that they could collide and cause problems[^4].

Should you want to see your MAC addresses on windows, just use `ipconfig /all` and apparently on UNIX its `ip addr show`.

We now know the answers two questions 1 and 2 on our Link-Layer home page

1. Who am I?

*Answer*: Check your MAC address for one way of defining yourself. 

2. If I know every device in my area, how will I make sure I send my message to the right person?

*Answer*: Send it to the correct MAC address.

Before we move onto question number $3$, there is something that you might have picked up on. What do we mean by "send it"? Sure we may know the MAC address, but how do we actually send it to the right person? This we will explain later in this chapter.

### No Wait Seriously Who Are You? (I Got Two Phones 🎵)

At this point we should address an elephant in the room that will not be discussed until Chapter $3$: your device actually has a second type of address called an *IP-Address*. The details of this are not important until later down the line, but what is important is that *IP-Addresses* in a LAN are kind of like names compared to MAC addresses that are like phone numbers. When you send a text message to someone, you want to send it in your messenger app using their name, so you don't have to memorize everyone's phone numbers, however phones don't understand names!

To go back to the story about playing Halo on a LAN, when you wanted to connect to the network, you would enter the IP-Address of the server in order to connect, not the MAC. IP addresses are easier to work with from a software and human perspective, but IP Addresses are Layer 3 addresses, so we need a way to convert a given IP address into a MAC address to send a message to.

> **Definition**: **Address Resolution Protocol (ARP)** (Defined in RFC 826[^5]) is the protocol that handles the conversion of L3 Network Layer Addresses into Link Layer Addresses. 

The intricate details of what goes inside of ARP packets is not useful for the purposes of this course, but the flow of how ARP communication works definitely is, if not because of how silly it is. 

> **Example**: Joe wants to send a message to Devin, but Joe's laptop does not know Devin's laptop's MAC. Show the ARP flow that will occur. Joe has IP address $(192.168.0.1)$ and MAC (11-11-11-11-11-11), and Devin has IP $(192.168.0.2)$ with MAC (dd-dd-dd-dd-dd-dd). 

1. Joe broadcasts the following frame/packet to every device on the LAN: 


[^1]: https://networkengineering.stackexchange.com/a/59970/101399
[^2]: Note that Halo $1$ was at least $12$ years old by this point, I wasn't *that* old.
[^3]: Technically one laptop acted as the server and everyone communicated with it, but thats not important
[^4]: NICs are also manufactured separately from the device they are attached to, and often are given their MAC at manufacturing time. As such they would not be able to coordinate giving the same MAC to all NICs on the same device anyway.
[^5]: https://www.rfc-editor.org/rfc/rfc826