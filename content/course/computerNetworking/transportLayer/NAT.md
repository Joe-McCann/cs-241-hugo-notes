---
title: Internet (but private) 
linktitle: Internet (but private)
toc: true
type: book
date: 2025-11-9
draft: false
tags:
    - cs356
    - transport layer
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 403
---

## Sooooo about those IP Addresses....

For our final discussion in the transport layer we are going to to wrap back around to the networking layer to discuss how we can use the transport layer to fix the problem related to the shortage of IP addresses. Really, the overall idea is quite straightforward, and something that people would want to do anyway. We are going to create a mini private network that goes through some main access point.

Effectively, we will split our network off from the main network. All of our devices inside our private network will be able to freely talk to each other, but no device from outside our private network should be able to initiate a connection with us. Rather if you want to communicate with a host outside the private network, you will initiate the connection, and the access point router will relay the communications between you.

The primary way of doing this is a super clever method called *NAT*.

> **Definition**: **Network Address Translation (NAT)** is a technique for using one IP address for many devices on a private network. 

The method goes as follows, the network will have one[^1] public IP address that can be accessed by anyone from the internet. All devices that live inside of the private network behind that router will be given a private IP address by the DHCP server. Some private IP address blocks are

1. `10.0.0.0/8`
2. `172.16.0.0/12`
3. `192.168.0.0/16`

as well as others. This is why you will very often see that whenever you connect to a new network, you always seem to get an address like this. This is because these are all private addresses on the internal private network, and whenever you communicate with the outside internet, the gateway router acts like a middleman in communicating your packets.

If these addresses are private, and thus globally non-unique, how can you actually communicate with the outside network? Every single packet that arrives for the private network, even if meant for different hosts, will use the IP address of the router. How will we be able to tell them apart?

The trick is that the router will fake using the port numbers as a way to address between different devices on the private network. Suppose our router has IP address `123.4.6.2`, and I have private IP address `192.168.0.2` with a socket running on port number `4523`. I will fire off my packet to some destination with my address tuple being `(192.168.0.2, 4523)`. Upon receiving this, the gateway router knows that `192.168.0.1` is a private IP, and thus cannot be sent onto the outside network and as such **changes** the IP in the packet to `123.4.6.2` and chooses a random port, lets say `1234`, to be changed as the port. The new packet after being sent from the gateway will have address tuple `(123.4.6.2, 1234)`, which will be added to the translation table


<table border="1" cellspacing="0" cellpadding="6">
  <tr>
    <th style="width:220px; text-align:left;">Private IP</th>
    <th style="width:120px; text-align:left;">Private Port</th>
    <th style="width:150px; text-align:left;">Public Port</th>
  </tr>
  <tr>
    <td>192.168.0.23</td>
    <td>6758</td>
    <td>10892</td>
  </tr>
  <tr>
    <td>192.168.0.3</td>
    <td>6542</td>
    <td>4444</td>
  </tr>
  <tr>
    <td>192.168.0.2</td>
    <td>4532</td>
    <td>1234</td>
  </tr>
</table>

Now when the access router receives a packet addressed to `(123.4.6.2, 1234)`, it can look at the table and know that its supposed to go to my host at `(192.168.0.2, 4532)`. 

However, what if we want to host a server inside of our private network that outside hosts can connect to? Infamously, this was the case for kids trying to set up minecraft servers in their parents house: because of NAT, nobody would be able to connect to you! This is when you need to apply a process called *port forwarding* which is a fancy name for just manually adding an entry into your NAT table.


[^1]: In practical uses, there will often be more than just one NAT Gateway, however we'll work in a simplified model where there can be only one.