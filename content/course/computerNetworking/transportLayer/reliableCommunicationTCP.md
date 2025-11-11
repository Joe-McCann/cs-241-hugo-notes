---
title: Communicating Reliably 
linktitle: Communicating Reliably
toc: true
type: book
date: 2025-11-3
draft: false
tags:
    - cs356
    - transport layer
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 402
---

## Wtf Did You Say?

You might have found it a bit strange that I literally said that the transport layer is all about making sure that we are providing reliable communication, and yet we did actually jack shit in UDP to make it so that our communication is reliable. 

How can we have reliable communication though? Lets build up some intuitive ideas, and then use those to describe one of the most famous protocols used across the entire internet. The early designers actually cooked some good stuff with TCP going to be so honest 🧑‍🍳

Normally, this is where I would theory craft the idea of a super cool transport layer protocol that's reliable, but I'll leave that to be a discussion 

## Transmission Control Protocol

The thought process of TCP is that we are working with a continuous byte stream that sometimes shuffles or drops our data. For this, we don't really care how that data is divided up into packets, when we give sequence numbers, the number **does not represent the packet number**, rather the sequence and ACK numbers represent the byte numbers that we are on.

TCP works through the idea of Acknowledged data, every time that data is sent, the send expects to receive back a new piece of data that acknowledges that previous piece. Specifically, TCP uses what is called "cumulative acknowledgements" which means that if we ACK all bytes prior to the number provided. For example, if I say `ACK 36`, this means that I acknowledge all bytes of data prior to byte 36, and am now ready to receive byte 36. As for why we do this we will see some advantages soon. 

Should an Acknowledgement not arrive, we will resend after waiting a specified timeout window. Normally I discuss more on how that window is calculated, but I don't think I will have time this semester.

First, let us provide the packet structure of a TCP packet, so we can observe and refer back to the terminology. 

## The Three-Way Handshake

In TCP, a "connection" is opened between the two hosts so that the two of them can send data back and forth between each other. In this sense, there is not necessarily a client and a receiver, because in each packet each host can send data while simulataneously acknowledging the previous data that it has received. 

For this reason, when we start up a new connection, not only does the initiator of the connection make a request to connect, but the receiver does as well. This is famously called the **three-way handshake**, and in a conversational way would go as follows between two people

> *Person A*: Hey I wanna call you to talk some gossip
> *Person B*: Sounds good, I also want to tell you some gossip
> *Person A*: Sounds good, can you believe that ....

Notice that both person A and B are informing each other that they each want to speak on some gossip while also acknowledging the fact that the other person has information they want to speak as well. In the three-way handshake it would technically go like this

1. Host A selects an initial sequence number $A_0$ and sends a packet to `(B_ip, B_port)` with $A_0$ as its sequence and the synchronize (SYN) bit set to $1$. 
2. Host B receives the synchronize request, opens a socket and TCP buffer for the connection, chooses an initial sequence number $B_0$, and sends a `(A_ip, A_port)` with $B_0$ as the sequence number, $A_0+1$[^1] as the acknowledgement number, and the ACK and SYN flags both set to $1$.
3. Host A responds to Host B by sending a packet with the SYN flag set to $0$, the ACK flag set to $1$, the sequence number of $A_0+1$, and the ACK number of $B_0+1$. A is also free to begin sending data inside of its data field.

The discussion of how we choose our initial sequence numbers will occur later on this page: its not necessarily $0$!

## Sending Data

After the three-way handshake has been established, we need to determine how we send data during a normal communication flow. Before sending though, we need to understand exactly how *much* data we actually can send. 

### Congestion Control and Flow Control

For an analogy, imagine my friend Ariana wants to send a video to my other friend Devin over text. The video is an hour long, so in order to make it more manageable she breaks it into $10$ six minute chunks. In theory, she could send every single on of those chunks at the same time, but what if that causes the network to overload? What if Devin runs out of storage midway? 

Rather, she should send a little at a time, and as Devin responds back letting her know he got them she can begin to send slightly more and more. This idea is what we use the *receive window* for. 

> **Definition**: The **receive window** is the amount of data that a receiver can handle, and is sent through TCP packets. The **send window** is the amount of bytes that the sender can send unacknowledged.

Note that the send window, and the receive window might not necessarily be the same, because the sender may be firing off data slower than the receiver could theoretically handle. Some problems with the receive window can be as follows
1. If the receive window reaches $0$, then the sender would never want to send more data. As such, if the window ever reaches $0$, the sender will periodically send pings to the receiver in order to get window updates.
2. If the receive window is very small and the sender continually sends data, then the small amount of data in the packets will not be worth the large TCP overhead (this is apparently called Silly Window Syndrome lol[^2])

TCP senders use a "sliding window" which means that the distance between the smallest and largest unacknowledged bytes cannot exceed the window. For a simplified example, imagine that we have TCP packets `1,2,3,4,5,6` that we want to send, with a send window of $3$. We send `1,2,3` all at once, but only get back the ACK for `2`. We *will not* fire off `4` yet, because we consider the window of `1,2,3` still not acknowledged. Once we get back the ACK for `1`, we can shoot off `4,5` and change our sent packets window to `3,4,5`. Of course note that this would not be split via packets in a real case, but rather through bytes. 

For the purposes of this class, the above situation can't really happen, because most TCP utilizes what are called cumulative ACKs, but some psychopathic variations do have scenarios like this occur.

So far, the idea of managing our send window to make sure we do not overload the buffer of the receiver is called **flow control**, however there is another type of control that we use to make sure that we do not overload the network called **congestion control**. In essence, we will be keeping track of two different window sizes, one for congestion and one for flow, and then our send window is the smaller of the two.

Congestion control is *extremely* implementation specific, but I will go over one easy way that is often used
1. Start with a single TCP packet as the send window and send it
2. If no problems occur (dropped packets, ICMPs, duplicate ACKs) then double the send window size. Repeat until problem
3. If problem occurs, halve the send window, and each iteration increase the send window by 1.

### Bufferbloat

Taking a quick dip back into the discussion about the Network layer[^3] (so scandy to bring up Network on a Transport page I know), we had briefly discussed about how routers had buffers to store incoming packets that were waiting inside of queues. These queues could form while both the packets were waiting to be processed, and while they were waiting to be transmitted to the outgoing link. As memory and storage became cheaper in the 2000s, people began to make routers have larger and larger buffers for queues: this was so that they could have less packets be dropped due to full queues, and thus less retransmissions, less traffic, and overall a faster internet. However, in some cases it appeared that increasing buffer sizes actually made people's `ping` speeds **slower**! How could this be possible?

Its possible to see how a large queue could lead to a slowdown. A longer queue means each packet needs to wait longer in line, but how is this really a problem if this was all data that needed to be sent anyway?

It turns out that TCP and its congestion control methods were to blame. Imagine that we have a WAP router that can process $2$ packets per second inside our house. We are playing a game of League of Legends that sends one packet per second, in normal times we would have no problems here. However, now your sister uploads a video to Google Drive, this process will be performed over TCP Sockets. If your queue is $1000$ packets long, through TCP congestion control, we will first begin to send `1`, then `2`, then `4`, so on and so forth packets at a time. Notice the problem is that our router can only actually handle $2$ packets a second, but because congestion control won't kick in until a packet is dropped our TCP sender will keep dumping more and more data! By the time the sender finally shuts up, the queue is way too long and will cause so much delay.

An analogy is when you are in a group talking, and one person won't shut up and the backlog of discussion topics will keep growing and growing. The solution for this is for one person to realize that this conversation is going on too long, and cutting them off so that they can take the hint. 

This is the idea of **Active Queue Management** or **Smart Queue Management**, in which a router understands what its throughput should be, and starts killing packets if it sees that one device is getting a little too talkative. In this way, a TCP connection would learn what the proper speed that data should be sent at is, sooner than on schedule.

In fact, when I turned on AQM (called QoS in my router) on my router at home, my ping speed from my house to Newark dropped from `50ms` to `15ms` 😰

### Cumulative ACKs

In most versions of TCP, we utilize what are called *cumulative ACKs*. This means that if I send out packets with sequence numbers `100, 200, 300` (remember this means that each packet would have $100$ bytes of data) then even if the ACKs for `100` and `200` get dropped, as long as I get the ACK for `300` (which would have ACK number $301$) I can mark all of that data as acknowledged because `ACK 301` means that everything prior to `301` has been received a-ok.

This also allows for senders to perform a "fast retransmit" instead of waiting for a timeout. In the event we receive four copies of the same ACK number, called a "triple duplicate ACK", we can be sufficiently sure that something is messed up and retransmit accordingly.

## Closing Connections

In TCP, there is a similar handshake to close a connection. The sender will initiate a `FIN`, which will be Acknowledged by the receiver, who will then send their own `FIN`. Upon receiving the acknowledgements each client will begin closing their connection. In the event the FINACKs are lost, retransmissions will be attempted until the connection is eventually killed and closed.

## Choosing Initial Sequence Numbers

## TCP Code

Here we will go over some simple TCP code in python to allow us to send messages between each other.

### TCP Client

```python
import socket

serverName = "127.0.0.1"
serverPort = 12503
clientSocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
clientSocket.connect((serverName, serverPort))
name = input("Name: ")
try:
    while True:
        message = input("Message: ")
        clientSocket.send((name + ": " + message).encode())
        message = clientSocket.recv(2048)
        print(message.decode())
finally:
    clientSocket.close()
    print("Connection Closed")
```

### TCP Server

```python
import socket

serverPort = 12503
serverSocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
serverSocket.bind(("", serverPort))
name = input("Name: ")
serverSocket.listen(1)
print("Starting Server")
connections = []

conn, address = serverSocket.accept()
print("Connected with ", address)
try:
    while True:
        message = input("Message: ")
        conn.send((name + ": " + message).encode())
        message = conn.recv(2048)
        print(message.decode())
finally:
    serverSocket.close()
    print("Socket Closed")
```

[^1]: We increase the ACK number by $1$ because we are acknowledging the first packet being sent which contains the syn. 
[^2]: https://en.wikipedia.org/wiki/Silly_window_syndrome
[^3]: Unfortunately, TCP and IP are linked pretty tightly because they were originally designed as one protocol. So yeah it sucks