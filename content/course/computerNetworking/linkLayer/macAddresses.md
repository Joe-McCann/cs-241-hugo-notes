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

## LAN Addresses 

If $A$ wants to send a message to $B$, how do we specify who $B$ is? Just like sending a text message to your crush, you have to provide some digits (address). For a phone this is a phone number, but for a LAN this is what is called a MAC Address. 

> **Definition**: A **Media Access Control Address (MAC)** is a $48$-bit network address used for the purpose of communication within a LAN. MAC addresses are typically notated in two-character hexadecimal strings: a6-01-b3-fd-23-cc for example.

MAC addresses **IN THEORY** are supposed to be globally unique, and thus in practice if you have two devices with the same MAC address on a LAN, the network is going to crash out. Since there are $2^{48}$ possible MAC addresses, this is a safe bet. MAC addresses are also assigned when a device is manufactured and are **IN THEORY** permanent[^6].

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

Another way to think about it is that IP-Addresses are location based (as we will see) and MAC addresses are permanent to the device. So it would be akin to when you move around, your name is permanent but your address will change; so there will need to be a way to understand who lives where and convert from address to person.

> **Definition**: **Address Resolution Protocol (ARP)** (Defined in RFC 826[^5]) is a protocol that handles the conversion of L3 Network Layer Addresses into Link Layer Addresses.

Suppose I (Joe) want to send a message to my friend Devin who is on the same LAN. I know Devin's IP address, but need to convert that into a MAC address that I can send to on my LAN. The first thing my device will do is check my device's **ARP Table** which contains records of what MACs go to what IPs.

A row in the ARP table will contain
1. An IP Address
2. A MAC Address
3. A Time to Live (TTL) of how long this record entry is good for

Since devices can enter and leave a network on a whim, we use the TTL to get rid of unneeded entries, and prevent problems from occurring if a device leaves the network, and its IP Address is reassigned afterwards. If you'd like to check your ARP table on your device try `arp -a` in your terminal.

If the IP Address we are looking for is not inside of the ARP Table, we will proceed with the ARP protocol. The intricate details of what goes inside of ARP packets is not useful for the purposes of this course, but the flow of how ARP communication works definitely is, if not because of how silly it is. In terms of important information, each ARP packet will have

1. A sender IP
2. A sender MAC
3. A destination IP
4. A destination MAC

For our following example, let us assume that 
- Joe's MAC is aa-aa-aa-aa-aa-aa
- Joe's IP is $192.186.0.1$
- Devin's MAC is bb-bb-bb-bb-bb-bb
- Devin's IP is $192.168.0.2$

The first thing that I will do when I realize that Devin's IP is not in my ARP table, is that I will build and send out the following packet. This first step in Wireshark is shown as `Who has?`
1. Sender IP: $192.186.0.1$
2. Sender MAC: aa-aa-aa-aa-aa-aa
3. Destination IP: $192.168.0.2$
4. Destination MAC: ff-ff-ff-ff-ff-ff

Since we don't know what the MAC address of Devin is, we set the destination MAC as the *broadcast* MAC, which means that this packet is sent to **every** device on our LAN. Upon receiving the packet, every device on the network receives it into their NIC, passes it along to the CPU which then checks the destination IP against its own. If the IP doesn't match, the CPU discards the packet and ignores. 

On Devin's device, the IP address will match, and thus Devin will send back the following response directly to me
1. Sender IP: $192.168.0.2$
2. Sender MAC: bb-bb-bb-bb-bb-bb
3. Destination IP: $192.186.0.1$
4. Destination MAC: aa-aa-aa-aa-aa-aa
Note that Devin will add my own IP and MAC to his ARP table, along with whatever TTL is set for new entries on his device.

### What is the Point?

It might be worthwhile to ask, "What is the point of having two addresses? Doesn't that just make this more complicated?" Eventually when we discuss IP addresses, we will see that since they are dynamic and provided upon entry to the network, they actually provide us some location information for networking purposes. Compared to MAC addresses that only give us device info.

This dodges the question though, why wouldn't we just use IP addresses for everything? Even LAN traffic, whats the point of having a second address? The reason is that in some types of networks, called broadcast networks, a device may receive every packet pushed to the network. As such the device will need to be constantly checking to see if packets are addressed to it. In busy networks this would put considerable strain on your CPU if you had to have it always checking against its IP address. As such, its worthwhile to have a separate MAC addresses so that the NIC can be in charge of filtering out messages.

Also, in this first protocol, we see already that multiple layers can be blended together. Is ARP an L2 or an L3 protocol? Its not clear!

[^1]: https://networkengineering.stackexchange.com/a/59970/101399
[^2]: Note that Halo $1$ was at least $12$ years old by this point, I wasn't *that* old.
[^3]: Technically one laptop acted as the server and everyone communicated with it, but thats not important
[^4]: NICs are also manufactured separately from the device they are attached to, and often are given their MAC at manufacturing time. As such they would not be able to coordinate giving the same MAC to all NICs on the same device anyway.
[^5]: https://www.rfc-editor.org/rfc/rfc826
[^6]: Modern iPhones for example have dynamic MAC addresses on startup.