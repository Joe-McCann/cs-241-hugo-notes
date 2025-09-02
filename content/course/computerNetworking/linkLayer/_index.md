---
# Course title, summary, and position.
title: The Link Layer
linktitle: Link Layer
summary: Getting From One Place to Another
weight: 200

# Page metadata.
date: "2025-08-22T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: book  # Do not modify.

---

__William Joe McCann__

### One Step at a Time, There's No Need to Rush 🎵

One layer above the physical layer, the link layer is actually deeply involved in the technical challenges of transmitting data across various mediums. Some of the many different protocols for different ways you may recognize connecting to networks (such as Wi-Fi, Bluetooth, and Ethernet) are L2 (Link Layer) protocols. 

Specifically, in the Link Layer we will concern ourselves with the following problem: If I wanted to *directly* communicate from my device to another device, how would I do that? What problems would we encounter? Note this is different than a network like the internet, which uses intermediary routing devices in between hosts, here we imagine we really only care about sending our data one step, which we will build upon in the Network Layer to take many steps in a row.

Here are some questions that we will discuss in this chapter:

1. Who am I?
2. If I know every device in my area, how will I make sure I send my message to the right person?
3. If devices can leave and join the network, how will I know who everyone is? 
4. How do we make sure multiple devices don't corrupt each others messages by talking at the same time by using an intermediate device? 
5. How do we make sure multiple devices don't corrupt each others messages by talking at the same time without using an intermediate device?
6. How can we figure out if a message has been corrupted?

Hopefully by the end of this chapter you can understand some proposed solutions to some of these ideas.
