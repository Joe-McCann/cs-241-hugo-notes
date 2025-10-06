---
title: The Internet Protocol Packet Structure
linktitle: IP Packet Structure
toc: true
type: book
date: 2025-09-28
draft: false
tags:
    - cs356
    - network layer
    - computer networks
    - internet protocol

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 302
---

## Buckle Up for IP 🚗

In the first of (hopefully very few) incredibly boring lecture topics, in this page we will discuss the finer details of the things that are contained within the IPv4 and IPv6 packets, and some of the differences between the two protocols. First, let us observe the IPv4 packet header that was designed long ago in 1981[^1].

{{< figure library="true" src="computerNetworking/network_page3_ipv4packet.png" title="IPv4 Packet" lightbox="true" >}}

After the header, the remainder of the packet is data. Here are some of the important fields in an IPv4 packet
1. Version: This is the version of IP being used, and is always set to $4$ in IPv4
2. Internet Header Length (IHL): Specifies the number of **words**[^2] that the header has of length, as the options field is variable in length. The mandatory header fields in total are $5$ words ($20$ bytes), and the max header length is $15$ words ($60$ bytes).
3. Explicit Congestion Notification (ECN): Allows routers to signal when their is congestion
4. Total Length: Total amount of data in bytes of the packet. Since it is $16$ bits long, this means that a maximum IPv4 packet can be $2^{16}$ bytes long total (including the header).
5. TTL: The time to live of the packet. This is measured in ROUTER HOPS, not time. This is $8$ bits which means at most a packet can survive for $255$ hops on the network before it expires.
6. Protocol: Refers to the Transport L4 protocol that is contained within the packet
7. Header Checksum: A $16$ bit checksum against the HEADER (not the data) to ensure there is no data corruption. Lmfao you're data is fucked but at least we got it to the right destination
8. Source / Destination Address: Addresses of the source and destination.

You might have noticed that I skipped over a few things. Unfortunately, the early internet designers were not fortune tellers, and had to kinda guess what would be important problems to deal with when going through the process of networking. As such they made some key errors.

### Problems with IPv4

The most infamous and glaring of all the problems regarding IPv4 is the address space. Each address is only $32$ bits long, meaning there is $2^{32}\approx 4,000,000,000$ possible addresses. While $\\$4$ billion dollars is certainly a lot of money, it absolutely not enough for the entire world, especially since individual devices can have many network interfaces which all take up their own IP address. Pretty quickly we realized that we were going to run out of IPv4 addresses, and the replacement IPv6 was formalized in 1998 as the successor to IPv4. Unfortunately, IPv6 is supposedly a little less than $50\\%$ of all internet traffic, even to this day.

However, less flashy is the problem with the *options* field. This is variable in length, which means every single time the packet reaches a router and checks / recalculates the IP checksum, it needs to perform an additional calculation to see where the header ends. This is just bullshit wasted effort, because normally nobody uses any of the options anyway. 

Worst of all though is the idea of IP fragmentation. The thought process is that different L2 protocols may have different max packet sizes they can handle, and even might be smaller than the max IP packet. As such, if a packet arrvies at a router that is too big for the next link, the router can fragment it into smaller pieces, send each additional piece to the next (or later) router, and then the receiving router can reassemble. Amazing in theory bad in practice[^3].

First problem with fragmentation is that it is intensive on the router to actually perform the fragmentation and reassembly operations, which can lead to network slow downs and data corruption if fragments are lost along the way. Even worse though is a fragment attack, in which an attacker will send incomplete IP fragments to routers on the network. These routers will wait for the remainder of the IP packets to arrive, but said packets will never come because the attacker is sending only pieces of the fragment.

## The Sixth Version of IP, but the Second Important One

Now let us show what the header of IPv6 looks like, which is optimized for processing speed, compared to IPv4

{{< figure library="true" src="computerNetworking/network_page3_ipv6packet.png" title="IPv4 Packet" lightbox="true" >}}

After the header, the remainder of the packet is *mostly* data, however some "optional headers" can be present afterwards. This is how IPv6 gets around the variable length headers, by including the unneccessary information outside the main header. Important fields

1. Version: Set to $6$ for IPv6
2. Traffic Class: Information regarding the importance of this packet, also contains $2$ bits for ECN from IPv4
3. Flow Label: In IPv4, if I am sending a ton of data across multiple packets for the same process/application, a router those packets flow through will think they are all just independent of each other. An application can provide a random number for all the packets it sends within the flow label so that any router that sees a sequence of packets with the same `(source, destination, flow)` can realize "oh, this data is all part of the same application". Should the router choose to do anything about that is up to the router.
4. Payload Length: Size of the payload (not including the headers) of the packet being sent.
5. Next Header: Specifies the type of the next header in the data so you can know whether there are additional options, or if its payload data.
6. Hop Limit: Same thing as TTL in IPv4[^4]
7. Source / Destination addresses: $128$ bit long addresses

Note that the address space is now $2^{128}$ addresses, which is fucking gigantic. IPv6 also no longer has a checksum, as the designers realized that both L2 and L4 have error handling, so its unnecessary.

IPv6 doesn't allow for fragmentation, and instead places the responsibility of making sure that you don't have too big of a packet on the sender, which makes sense. When you literally mail a box, you make sure its within the proper weight range before sending, each mail carrier doesn't individually check the weight to see if its good and split it as needed into smaller boxes. However, how would a host actually know what the maximum allowed packet size on the path would be? In order to determine this we need another type of L3 packet

## Internet Control Message Protocol (ICMP)

Oops tricked you into thinking that this page was only about IP packets but haha now we are doing ICMP packets too.

The early internet designers realize that it might be a good idea to allow routers to send their own messages to communicate when things are going wrong, so in the same RFC as IP (791), they introduced Internet Control Message Protocol, which is a protocol for tiny messages that are passed around the internet for managerial purposes.

For an analogy, if at a restaurant all the customer-server communications are the IP packets, then ICMP packets are all the messages that are communicated between employees regarding management matters. For example, the waiter might ask the chef to prioritize the food of a particularly unruly table, or a waitress may inform the table that a particular item is out of stock for the night. These messages are more so to make sure everything runs smoothly, rather than the actual data we care about.

{{< figure library="true" src="computerNetworking/network_page3_icmp4packet.png" title="ICMP Packet for IPv4" lightbox="true" >}}

This is specifically the ICMP packet for IPv4, and there is a corresponding ICMPv6 packet[^5] but we will just stick to v4 for our purposes. Fields of ICMP are
1. Type: What type of message is prompting this packet to be sent
2. Code: Sub-category of the specfic type. Type+Code together provide all details of what error occured
3. Checksum: $16$ bit checksum
4. Rest of Header: Remainder of header is actually variable depending on the Type+Code

After the ICMP header, a data portion is sent that contains a copy of the header, and the first few bytes of the IP packet that triggered this ICMP to send. Some of the more interesting ICMP reasons are

1. Type 0, Code 0: Echo **reply** used for `ping`
2. Type 3, Code 1: Destination Host unreachable
3. Type 3, Code 4: Fragmentation Required (packet too big for link), but packet does not allow fragmentation 
4. Type 3, Code 7: Destination Host unknown
5. Type 4, Code 0: Source Quence (ie STFU) to state that you are sending too much data. This is deprecated
6. Type 8, Code 0: Echo **request** used for `ping`
7. Type 11, Code 0: Time Exceeded, TTL has reached $0$

With the ability to read ICMP messages, we can do some fun things such as

### Trace Route

As mentioned before, the `tracert` command allows us to see all the routers that our packets are taking along a route from one node to another[^6]. By executing `tracert [insert website name here]` in your command line, you will get to see the IP addresses of all the nodes on your path. 

The way this works is by firing off `echo` requests to the destination with increasing TTLs. To start you will send one with a TTL of $1$, so that it expires at the first hop. When it expires you will receive back an ICMP with type `11` and code `0`, and can write down the IP of the device that killed the packet. Then you send a request with TTL of $2$, so that the second hop will kill it and send you back an ICMP of `11` and `0`. This continues until you eventually get back an echo reply and you know you are completed.

### Path MTU Discovery

Previously we said that IPv6 (and most modern IPv4 applications) require that the host perform the fragmentation of its packets in the event that the packet sizes are too big for the links on the path. As such, we need a way to know what the smallest Maximum Trasmissable Unit (MTU) of the path is; aka what is the largest packet size that will fit on every single link of the path.

The algorithm, called Path MTU Discovery (PMTUD), for this is straighforward:
1. Start by sending a packet of a size that is the MTU of the link connected to the host
2. If an ICMP of Type 3 Code 4 is sent back, this means some link was too small for the packet
    1. ICMP Type 3 packets contain the next hop MTU, set the new Path MTU to be this value and resend the data using this MTU.
3. Repeat until the data arrives at the destination without problem

This process requires that for IPv4 that you set a bit that explicitly notes to not fragment the packets. In IPv6 there is no fragementation so it doesn't matter.


[^1]: [RFC 791](https://www.rfc-editor.org/rfc/rfc791)
[^2]: One "word" is equal to $4$ bytes or $32$ bits.
[^3]: https://networkengineering.stackexchange.com/a/64804/101399
[^4]: Not really relevant, but I found this rando dude's [blog post](https://techstat.net/1-1-d-iv-ipv4-ttl-ipv6-hop-limit/) about this and he's so salty I'm cackling. *"I find that they are reusing Type and Codes quite annoying as it makes remembering these more difficult. We have an infinite list of numbers to choose from and they had to re-use the same numbers."*
[^5]: https://www.rfc-editor.org/rfc/rfc4443
[^6]: Technically it is not guaranteed that all packets take the same route, but its close enough