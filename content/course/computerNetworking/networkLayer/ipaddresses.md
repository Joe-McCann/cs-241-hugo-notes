---
title: Addresses (Again)
linktitle: Addresses (Again)
toc: true
type: book
date: 2025-09-28
draft: false
tags:
    - cs356
    - network layer
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 301
---

## Where do we go from here? 

In the previous chapter we discussed the idea of a MAC address, which allowed us to send messages to devices on our own LAN. The main purpose being so that a network interface could know when link layer packets were being sent to it, and filter out any unnecessary noise, as well as being used for L2-Switch routing.

In this way, a MAC address is kind of like the name of a person who lives in a specific dorm building. If mail is received to that dorm floor, than the RA can pass it off to the correct person because they know everyone. The name of that person is identifying information about that specific *person*, but if someone wants to send mail across the country, they don't actually send it to person, rather they send it to a *location*. 

If your girlfriend who totally lives in a different school was to send you a letter, she would address it to the address of the location that you are currently living in. Note that this is different than sending it to you, yes it is *your* dorm, but the address of the place you live in can change over time, and the person who lives at that address can change over time[^1]. This is the idea of the famous *IP Addresses*.

> **Definition**: **Internet Protocol (IP)** is the most popular protocol used to route data from one node in a network to another, and its address values used are called **IP-Addresses**.

IPv4 Addresses are $32$ bits long and are formatted in groups of $4$ bits like `221.54.1.3` where `221` represents `11011101`, for example. We will be discussing from the perspective of IPv4 for most of the class, however IPv6 is growing in popularity and has addresses that are $128$ bits.

IP is designed to provide a "best effort" delivery service of your message, because the original designers did not want to put any additional overhead into networks or applications they were not aware of. Maybe your links are unreliable, maybe they aren't. Maybe you need speedy service, maybe you don't. Those things are the application's concern, not the network's. As such "best effort" means that you get

1. No guarantee that your message arrives at all
2. If it does arrive, no guarantee it arrives uncorrupted
3. No guarantee it arrives within a time window
4. No guarantee that multiple packets sent will arrive properly in order

Gonna be honest, thats some really shitty service.

### Dynamic Host Configuration Protocol (DCHP)

Before we discuss what happens with respect to IP addresses and how the Network layer uses them, lets first begin by discussing how a device actually gets an IP address in the first place. One way to do this is to manually have a network admin provide every device an IP address when they enter the network, but that would be inefficient and suck. As such, one solution is to use *DCHP*.

> **Definition**: **Dynamic Host Configuration Protocol (DCHP)** is a protocol that provides new devices entering into a network with an IP Address. 

DHCP works with two types of devices: a DHCP server that provides IP Address, and a DHCP client that requests an IP address. DHCP servers can be found inside of home routers, campus[^2] networks, or large swaths of ISP zones. It is possible for multiple DHCP servers to service the same network, if there is a potential for a large amount of traffic.

In a simplified view, DHCP goes through four stages: 

1. **DHCP Discovery**: The client broadcasts to everyone by using the `255.255.255.255` broadcast IP address a request for an IP address.
2. **DHCP Offer**: All DHCP servers that receive the broadcast will send an offer for a specific IP address to the client. The offer contains an IP Address, as well as other important network information such as the IP/MAC of the first hop router, the subnet mask (which we will discuss soon), DNS server IP (application layer discussion), and an IP TTL. Depending on a flag set by the client in the *discovery*, the offer can be either a broadcast or a unicast[^3] chosen by if the client is OK to receive a packet with the IP set in its destination[^4].
3. **DHCP Request**: The client choses one specific IP address and sends a broadcast with the selected IP, as well as which server sent the selected IP. All other servers note this down that their sent IPs can still be used later.
4. **DHCP ACK**: The selected DHCP server responds to the client acknowledging that the IP address has been chosen. The client knows it can now use this IP address.

Also, it should be noted that before sending the offer, a DHCP server will often send an ARP or ping to the IP address it is sending out in order to see if that IP actually is free to provide.

[^1]: Mail for the previous tenant still arrives at my apartment four years later.
[^2]: Campus refers not just to colleges, but corporate campuses too.
[^3]: Unicast is the opposite of a broadcast - you just send to one device
[^4]: Because at the point of the offer, the client still has not chosen an IP address, so how would the client know what the correct IP is to listen for? Spoiler: it doesn't! It shouldn't matter though because the DHCP server knows the client's MAC and can send that way. 