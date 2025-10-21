---
title: De-Centralized Routing Algorithms
linktitle: De-Centralized Routing
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
weight: 304
---

## Everyone's the Captain Now

In the previous section, we detailed how centralized link-state algorithms, such as Dijkstra's, can be used to find the shortest path from one node to another one. However, in the early days of the internet this was not the desired design. There were legitimate concerns about the stability of such systems, and as such wanted an entirely de-centralized system, that could still maintain itself in the event that something went wrong (like a node failure)

### Bellman-Ford Algorithm

The algorithm that we will go over that is a *distance vector* type algorithm is called the "Bellman-Ford" algorithm. This was the first implemented algorithm for the purposes of routing. 

Heres a video. Im too sad right now to make proper notes Im sorry: https://www.youtube.com/watch?v=z-3cfz1g9Wc

### Border Gateway Protocol

Yeah man breakups suck. Each ISP has control over their own tech and actually don't want other companies knowing whats going on inside of there. As such they have full control over their own zone, and actually independently route inside of it, this is called an *autonomous system* (AS). Each AS needs to know about subnets that may not actually be inside of their own AS, an AS will broadcast to its neighbors all of the subnets it contains, and those AS will broadcast to its neighbors. 

If there are multiple AS that a route can take to go to the desired destination, then internal business rules will apply to make the decision. My favorite is *hot potato* routing, which means get the packet off our AS as fast as possible. Similarly there is *cold potato* routing.

