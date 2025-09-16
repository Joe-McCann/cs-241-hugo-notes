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

Lets consider an analogy to explain what is going on here. `Insert analogy that I torture my students to provide`

A switch receives a message, and sends it to the proper cable. Awesome. Easy. However, how does the switch know which cable to send it to? Just like in ARP we have a table which lists MAC addresses with corresponding interfaces

<table border="1" cellspacing="0" cellpadding="6">
  <tr>
    <th style="width:220px; text-align:left;">MAC Address</th>
    <th style="width:120px; text-align:left;">Interface</th>
    <th style="width:150px; text-align:left;">TTL (seconds)</th>
  </tr>
  <tr>
    <td>00-1a-2b-3c-4d-5e</td>
    <td>1</td>
    <td>1200</td>
  </tr>
  <tr>
    <td>00-1c-42-2e-60-4a</td>
    <td>2</td>
    <td>1150</td>
  </tr>
  <tr>
    <td>00-1f-29-3b-7c-8d</td>
    <td>3</td>
    <td>900</td>
  </tr>
  <tr>
    <td>08-00-27-ab-cd-ef</td>
    <td>4</td>
    <td>600</td>
  </tr>
  <tr>
    <td>3c-97-0e-12-34-56</td>
    <td>5</td>
    <td>300</td>
  </tr>
</table>


provides an example of what this table could look like. In here, if our switch received a packet that had a destination MAC of 00-1a-2b-3c-4d-5e, the switch would know that it had to fire it out of the connection on interface $1$. 

### Filling Up Switch Tables

That makes sense if a switch table has the entry that we are looking for, but how do we fill up the entries of the table? Also, what happens if we want to send to an address that's not in the table?

The tables are wonderful because they are *self-learning* in that there is actually no outside intervention required in order to make them work, which would make your life as a network administrator very easy. Not only this, but it also helps make the switch invisible to hosts, as hosts do not need to intervene at all to make the switch work.

 For all link layer protocols, the packet will have to have both a `sender` MAC and a `destination` MAC listed. When the switch picks up an incoming packet on one of its interfaces, it reads the sender MAC, and adds that along with the interface as a table row. 

You can imagine the switch having the following internal monologue: *"Welp, I just received a message from 11-11-11-11-11-11 coming in from door $9$, so let me write down that if I ever need to send them a message, to fire it out of door $9$"*. By the way, note that even when many switches are connected in a network together, this process will still apply. We will go over an example on this later.

We are now ready to answer the final question of, what do we do if the switch receives a packet, but it doesn't recognize the `destination` MAC address? This is easy, just send it to everyone other than the sender! All devices that are not the intended recipient will discard the packet, but the recipient will the process, and respond accordingly. When the destination host sends back its response, the proper entry will be added into the table.

### Table and ARP Security ☠️

Link Layer Switch tables and ARP (discussed in previous page) were not designed with security in mind, and thus are susceptible to attacks from people looking to be evil. The type of attacks that these solutions are most susceptible to are `Man-in-the-Middle` attacks, in which an attacker secretly receives all traffic intended for someone else, before properly passing it along. With this in mind, the attacker could block all your traffic, read all your traffic, or even modify it.

In what is called `ARP Spoofing`, an attacker will send out fake ARP packets with your MAC address, to get network switches to send the traffic in their direction instead of yours. It is also possible for an attacker to hijack incoming ARP requests so that other devices mistakenly believe them to be you. 
