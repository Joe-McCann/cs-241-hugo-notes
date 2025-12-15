---
title: Domain Name System - Who Are You?
linktitle: DNS
toc: true
type: book
date: 2025-11-17
draft: false
tags:
    - cs356
    - application layer
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 501
---

## Whooooo Are You? 🎵

When you go into your phone to text someone, do you enter their phone number every single time? Absolutely not, that would be a nightmare[^1], so we have software on our phones where we can enter the name of a person and it will be converted to a phone number for us: contacts.

IP addresses on the internet work the same way, if I want to make a request to a server, my packets need to have IP addresses. However, I do not have the addresses for `google.com` or `youtube.com` memorized. Rather, I just remember the names of these places, and my browser takes care of converting the names of these services into IP addresses. 

> **Definition**: A **domain name** is a human readable name that refers to a application on the internet.

Its very important to note that a domain name does not have a one-to-one correspondence with hosts, for example, `google.com` has many, many, many servers that all do the same thing, and the domain name refers collectively to all of them. What we would like, is for a service that we can provide a domain name for an application, and it will send us back an IP that we can find that application at. Luckily, there is a service that can do this

> **Definition**: The **domain name system** (DNS) is an application layer service that converts domain names into IP Addresses. 

In reality, DNS actually does more than just convert domain names into IP addresses, some of its other functionality include

1. Load Balancing by sending many requests for the same application to different servers
2. Aliasing to allow multiple different domain names to all refer to the same "canonical" domain name[^2] (aka c-name)

In the case of the second one, sometimes domain names are themselves really long and annoying to remember, so we make a domain name for our domain name that's even shorter 😂. A good example is my own website here, you can type in `wjmccann.com`, but in reality that is not the domain name for this service which is actually `<fill in it here>`

Currently, I am not super concerned with giving the details of what a DNS request looks like, and as such am fine with y'all just understanding that if you input a domain name, you can get back an IP Address or Canonical name as a response. The process for a DNS request looks as follows:

1. Application on host has domain name that it wants to convert into IP Address
2. DNS client on application host makes a request to a DNS server to resolve the given domain name
3. DNS server responds to DNS request and provides back IP Address or c-name

DNS runs over UDP and servers are on (normally) $53$[^3]. It is done on UDP as it is on the client to understand when they should reasonably make a re-request, rather than waste all that bandwidth on TCP overhead.

### The DNS Heirarchy

The DNS as a whole is the most traffic intensive service on the entire internet in terms of number of requests[^2] and thus implements several methods to reduce the strain on the system and make it as efficient as possible. 

The first optimization is caching, in which common IP addresses are saved within the DNS client or certain DNS servers, so that calls can be avoided if that is what the user is asking for. However, outside of this, DNS works in a heirarchy that gets more specific in order to ensure that not every server needs to keep track of every IP Address and c-name. The heiracrchy goes as follows

1. Root server: Approximately $1000$ of them across the globe, act as first point of contact for all DNS queries (sort of)
2. Top Level Domains (TLDs): Domains such as `.com`, `.net`, and `.edu`. Each country also has their own TLD such as `.gov` for the US, `.fr` for France, `.tv` for Tuvalu, and `.ai` for Anguilla. 
3. Authoratative Name Servers: Servers that house the information on IPs for a specific application, such as `.aexp` for American Express or `.njit` for NJIT. Every application that has public facing IPs must have an authoratative name server[^4]. 
4. Local Name Servers: Servers that handle queries by users within a organization's network. IPs of these name servers are provided through DHCP and they handle contacting the Root Level Server for queries originating within the org. 

This is also how you read IP addresses! If you see `web.njit.edu/mypage`, you can see that after hitting the Root level server, you would then be directed to the `.edu` TLD, which would then direct to the `.njit` authoratative server, which would then serve back information on the requested site. On the next page we will discuss what the `/mypage` part means. 

When making a query, we can request that this query will be either *recursive* or *iterative*. 

1. Recursive requests mean that the requested DNS server will actually make the request to another server itself if it does not have the proper record. The originally requested server will return back the resolved IP Address. This is akin to telling the requested server "I don't care what it takes to get it, just give me the IP" 
2. Iterative requests mean that if the requested server does not have the record, then it will return back the IP of the next server in the chain that might have it. The requester will then fire off the next request to the appropriate server. This is akin to the DNS server saying "I don't have it, but this guy might. Please leave me alone"

Normally, when you request your Local DNS server, you will make a recursive request, because your goal is to get the IP. However, your local DNS server will make it iterative to avoid adding additional load on the higher level servers. 

[^1]: Despite that being how people used phones back in the day, having "roledexes" or just memorizing peoples' numbers 
[^2]: I have no source to back that claim, however I will make the logical argument that almost every other request to a service on the internet must first go through DNS in some capacity (whether local, root, or cached). And this makes sense to me.
[^3]: Which is not a prime number
[^4]: If you host a website on a service like Squarespace, your website will be under their authoratative name server. My website is hosted by netfly for example. 