---
title: Centralized Routing Algorithms
linktitle: Centralized Routing
toc: true
type: book
date: 2025-10-12
draft: false
tags:
    - cs356
    - network layer
    - computer networks
    - internet protocol
    - routing

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 303
---

## I am the Captain Now

In order to motivate the next couple of algorithms that we will discuss, it is important to first explain what the routers actually do in order to send out a packet that comes in. When a router receives some packet, it will first perform the necessary IP steps
1. Check the IP Checksum (if IPv4)
2. Adjust the TTL
3. Recompute the IP Checksum

but then it needs to understand where is the best place to send this packet to. If a router has hundreds of interfaces, which one should we send the outgoing packet to? 

Inside the router there is something called a *forwarding table* which is like an L3 version of the L2-Switch tables. The entries inside of this table are subnet masks along with corresponding interfaces. When trying to determine where to send the packet, the router will compare the destination IP address against all the subnets in the table, select whichever subnet mask has the **longest** match, and then send it to that interface. We state longest match, because if the router had entries `192.0.0.0/8` and `192.168.0.0/16`, then we would match `192.168.0.1` to the second option, as we would assume that the more specfic entry will provide a better route.

Note, any packet that finds *no* matches is discarded.

With this in mind, we need to find out ways in which we will actually be able to compute all of these routing tables. In this page we will discuss the idea of a *centralized* routing algorithm, and then later we will find a way to do a decentralized algorithm

> **Definition**: A **routing algorithm** is an algorithm that finds the optimal path between two points for usage of calculating forwarding tables. A **centralized** algorithm performs all the computations from one main node that has a full view of the entire network and all conditions, whereas a **decentralized** algorithm performs computations individually from each node, with only the information of the device itself and all its neighbors.

## Graph Theory

For these two pages, we will need the basic language of graph theory to discuss out algorithms. Eventually this will have its own segments for discrete mathematics, but I'll put the info you need to know here for brevity. We assume for this page that you are familiar with the concepts of sets.

> **Definition**: A **graph** $G$ is a pair $\left(V, E\right)$ where $V$ is a set of **vertices** (aka nodes) and $E$ is a set of **edges**. If $G$ is an **undirected** graph, then each edge is a set that contains the two vertices connected to the ends of that edge, so $\left\{v_i, v_j\right\}\in E$ where $v_i, v_j\in V$, meaning that every edge is a bi-directional connection between two vertices. If $G$ is a **directed** graph, then each edge is a tuple which moves from left to right, so $\left(v_i,v_j\right)\in E$ with $v_i,v_j\in V$ means that $v_i$ connects to $v_j$, but $v_j$ does not necessarily connect to $v_i$. 

For our purposes, we can consider the routers and hosts (any L3 or higher device) our "nodes" and the links between them our "edges". We can also assume that our graph will be undirected, but that might not always be the case. With this definition alone we can come up with some cool theorems and properties (which I will spare you from), but we need some more in order to determine how we are going to compute the distance between nodes in our network graph. 

You see, when we want to find the shortest distance between two nodes, its not always going to be the case that we want to minimize the number of hops we make in our route. Perhaps one particular link is very slow, compared to a sequence of $3$ links that are all super fast. In certain situations, the $3$ link path might be more appealing! As such we need to quantify how "costly" a link would be to take in our graph.

> **Definition**: A **weighted** graph $G=\left(V,E\right)$ is a graph that also has a weight function $w:E\rightarrow \mathbb{R}$[^1] that takes as input an edge and returns a numerical value of that edge's "weight" or "cost".

These weighted graphs are often represented by drawing them with the weights over their respective edges. Now that we know what a weighted graph is, lets find a path

> **Definition**: A **path** between two vertices $a, b\in V$ is a connected sequence of edges in $E$ $$(a,v_1)(v_1,v_2)\ldots(v_k,b)$$ that starts at $a$ and ends at $b$. The **minimum** path between $a,b$ on a weighted graph is the path with the smallest sum of the weights of the edges.

This is exactly what you would imagine a "minimum path" to be, and likely what you are already familiar with. For each individual router, we are going to want to know what the edge we need to exit from should be in order to go down the minimum path for a specific destination, but how do we do this?

## Dijkstra's Algorithm

The holy grail of centralized routing algorithms is Dijkstra's algorithm. In order to compute this, a node needs to have a full picture of the entire network and its conditions (which will make up the weights). Things like congestion, link speeds, and distance can all be factored in together in order to determine the weights of certain links for routing purposes.

Traditionally, centralized algorithms were very *very* frowned upon, because the idea of having a single point of failure was one of the big things the designers of the early nets were trying to avoid. Nowadays, networking is reliable enough where a centralized server within the ISP can perform this calculations, and if something was to go wrong, one of several backup servers could kick in. As such, the main server is in communication with the routers on the network, constantly getting and giving updates regarding what the plan should be.

[^1]: This notation means that $w$ is a function that maps edges in $E$ to real numbers $\mathbb{R}$