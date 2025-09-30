---
# Course title, summary, and position.
title: The Network Layer
linktitle: Network Layer
summary: Mr. Worldwide 
weight: 300

# Page metadata.
date: "2025-09-26T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: book  # Do not modify.

---

__William Joe McCann__

## Traveling Around the World

Previously in the Link Layer, we discussed the mechanics for how we would move from one node to another within a LAN, and how we would deal with some problems that can occur during that process. In the Network layer, we move on from the idea of moving from one node to another next to it (as we assume now we can do that) and answer the question of how do we find a path between any two nodes in the entire network. 

A more succinct way of saying it is "the link layer tells us how to move from one node to the next, but the network layer is the one that picks what the next node is". Note that from here out we will call our switches **routers** to signify their places as L3 devices

Just like the link layer, we will encounter some problems that we will need to address in this chapter

1. How does a router determine what interface to send a message out of?
2. How do routers pick the "best path"?
3. How do we identify hosts and groups of hosts? 
4. How do we make sure that everyone on the internet cooperates? 