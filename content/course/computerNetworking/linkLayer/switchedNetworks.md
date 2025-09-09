---
title: Switched Networks
linktitle: Switched Networks
toc: true
type: book
date: 2025-08-26
draft: false
tags:
    - cs356
    - link layer
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 202
---

## How Do We Actually Send Messages to People?

In our previous page, we said in a hand-wavy way that we "send" messages to people via their MAC address. But how do we actually send these messages to people? The way that you are most likely familiar with is in a way called a "broadcast network" in which every message is fired for everyone to listen to. This is how it works in the example of Wi-Fi, you transmit the message, everyone hears it, and only the relevant people action on it. 

These networks have *many* problems that need to be solved though, so we will first start with a much simpler, albiet more modern, way of sending messages to the correct people. Can't overwhelm y'all already :sweat_smile:. 

We are going to take advantage of a type of packet switching device called a **Link-Layer Switch** or an **L2 Switch**. L2 Switches are fundementally so easy that it should be no problem for you, in fact, to hosts L2 Switches are invisible! Every host in the network is directly connected to a switch (normally via Ethernet), so every time a host sends a message, the switch receives it first. Upon receiving, the switch determines which link connected to it connects to the destination host and then forwards it accordingly.

Lets consider an analogy to explain what is going on here. Imagine in your dorm building (honors, Cypress, etc.) that there was a service that ran messages between rooms. So if you wanted to send a secret letter to your friend, you could slide it under your door, the message runner would pick it up, and then would slip it under your friend's door. 
