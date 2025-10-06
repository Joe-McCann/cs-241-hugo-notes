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
    - internet protocol

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

1. **DHCP Discovery**: The client broadcasts to everyone using `255.255.255.255` for a request for an IP address.
2. **DHCP Offer**: All DHCP servers that receive the broadcast will send an offer for a specific IP address to the client. The offer contains an IP Address, as well as other important network information such as the IP/MAC of the first hop router, the subnet mask (which we will discuss soon), DNS server IP (application layer discussion), and an IP TTL. Depending on a flag set by the client in the *discovery*, the offer can be either a broadcast or a unicast[^3] chosen by if the client is OK to receive a packet with the IP set in its destination[^4].
3. **DHCP Request**: The client choses one specific IP address and sends a broadcast with the selected IP, as well as which server sent the selected IP. All other servers note this down that their sent IPs can still be used later.
4. **DHCP ACK**: The selected DHCP server responds to the client acknowledging that the IP address has been chosen. The client knows it can now use this IP address.

Also, it should be noted that before sending the offer, a DHCP server will often send an ARP or ping to the IP address it is sending out in order to see if that IP actually is free to provide.

#### Broadcast Zones

Of course, a ton of devices are firing off DHCP requests all the time across the internet, if every single one of those was broadcast to every device then there would be no possible way of literally any other traffic occurring. As such, broadcasts that your devices fires are limited to a smaller subdivision of the network, called a "broadcast zone".

> **Definition**: A **broadcast zone** is the collection of all devices that can receive each other's broadcasts on a network.

For our purposes, we will say that the broadcast zone of our access networks is everything up until the first router. 

In the event that the DHCP server is actually behind the first router, the router will pick up the broadcast and directly send it to the DHCP server as an intermediate for communication.

## Subnets

When I list out my childhood home address, I would say that it is "801 Darlington Drive, Old Bridge NJ, USA". Reading that address from left to right, we actually gain information of increasing degrees of precision on where the place is located. First, we see that it is in the USA, therefore if we were to bucket all the addresses of all the places in the world into collections grouped by country, this one would go into the USA bucket. 

Perhaps that bucket would have a label like "xyz, USA" or "_________, _______, USA" to denote that there are these blanks that all the addresses inside of our USA bucket would need to fill. Not just that, but any mail carrier that needs to figure out what bucket to send a piece of mail to would just check the last set of characters and see "oh, thats USA, I'll put it inside of the USA bucket". Also, if two people were talking and shared their addresses, both could see how much location they have in common just by comparing the ends of their address descriptions. For computer networks, we would call this a **subnet**.

> **Definition**: A **subnet** is a collection of nodes on a network with shared IP-Address Prefixes.

So unlike traditional addresses that go from right to left in terms of getting more specific, IP addresses start general on the left and become more specific on the right. 

So if we had the IP addresses: `132.145.0.0`, `132.145.0.128`, and `132.145.0.12`, we could see that all of these devices should be a part of the `132.145.0.x` subnet, where `x` represents the remaining bits that are filled in by the devices on the subnet. Instead of providing our subnets though with `x`s, we will use a special notation to represent this

> **Definition**: A **subnet mask** is a $32$ bit sequence that represent what the subnet part of an IP address is, with the remaining bits being set to $0$. Notationally the subnet IP is written, followed by a / and the number of relevant bits called Classless Inter-Domain Routing Notation (CIDR)

The subnet mask of our previous example would be `132.145.0.0/24`, which represents that the first `24` bits are all parts of the address that are being used by the network part of the address, and the remaining `8` bits are available for the devices to use. This is the same as if I had provided a blank slot for people to place their street addresses in my neighborhood "____ Darling Drive, Old Bridge NJ, USA". 

When doing subnet masks though be careful, these are based on the BITS, not the numerical representation of the IP address, so it is totally possible for the subnet mask to not split evently on a dot.

> **Example**: Find the longest possible subnet mask of `89.211.82.16`, `89.211.70.2`, and `89.211.91.164`.

{{% callout info %}}
<details>
<summary>Answer</summary>
Immediately we can see that all three hosts share the same first $16$ bits of `89.211`, however since disagreements begin in the third segment, we need to investigate that segment more directly. In binary, $82=01010010$, $70=01000110$, and $91=01011011$. As we can see, the first $3$ bits of all of these numbers are the same, and thus those three bits are going to need to be included in our subnet mask.


Writing this out with $x$s as placeholders, we could say that our mask would be `89.211.010xxxxx.x` (expanding the third set of bits), so we see that the first 19 bits of the address would be our mask, in true notation (setting all the $x$s to $0$s) we would have our subnet mask to be `89.211.64.0/19`.
</details>
{{% /callout %}}

As a follow up question to our example, we can see that the first $19$ bits are part of the subnet mask, so the logical next question would be how many possible devices can be on this subnet? Since an IP Address is $32$ bits, if we are using $19$ of them, then we would have $13$ remaining. This means there are $2^{13} = 8192$ possible addresses. In reality there may be less available, as some subnets will actually have their own unique broadcast address, which is just the subnet mask followed by all $1$s.

### Default Gateways

In DHCP, one of the pieces of information provided is called the *Default Gateway* which is the router that allows you to exit our network and enter onto the wider internet. This is also often called the "first hop router". For the purposes of connection between previously L2 and now L3 material, we can see that anytime we want to send a packet across the internet, we will perform

1. Check the destination IP address against our subnet mask, if within the subnet, send directly
2. If on a different subnet, then send packet using the MAC address of the default gateway
    1. If you do not have the MAC address of the default gateway, then make an ARP request using the IP Address of the default gateway

That final point is one of (but not the only!) the reasons why the default gateway needs an IP address, because ARP requires both an IP and MAC address to work. 

Finally, similar to MAC addresses, each network interface will have its own IP address. This means that a device may actually have many IP addresses (and in fact, one router alone can take up a ton!).

[^1]: Mail for the previous tenant still arrives at my apartment four years later.
[^2]: Campus refers not just to colleges, but corporate campuses too.
[^3]: Unicast is the opposite of a broadcast - you just send to one device
[^4]: Because at the point of the offer, the client still has not chosen an IP address, so how would the client know what the correct IP is to listen for? Spoiler: it doesn't! It shouldn't matter though because the DHCP server knows the client's MAC and can send that way. 