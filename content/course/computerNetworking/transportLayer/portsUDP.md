---
title: Device to Device Communication
linktitle: Device to Device Communication
toc: true
type: book
date: 2025-10-26
draft: false
tags:
    - cs356
    - transport layer
    - computer networks

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 401
---

## Can You Hear Me Now?

As we learning in the network and link layer, it turns out that our underlying network is actually quite shit at actually getting the message that you are sending to the destination. Routers can just murder your data for seemingly no reason, and you device often has to get its message nuked for reasons completely out of its control (how was I supposed to know that there was traffic in this route??). With this in mind, hosts on our network need to use this internet super highway as if we were sending messages in war and we have no way of knowing if our carrier pidgeons are going to get shot down. Specifically this is called the [Two Generals Problem](https://www.youtube.com/watch?v=IP-rGJKSZ3s)[^1].

Good old mathematics getting in the way, it turns out that it is impossible for both devices to be completely in sync about what the other device knows. There will always be some information that we are unsure about. As such, we can only do our best, and the protocols we will show will illustrate varying levels of trying our best.

Since the underlying network can be (mostly) thought of from now as a magical device that just makes stuff work, we can consider the analogy of a couple sending love letters to each other in a long distance relationship: one of them is in Honors, and the other in Laurel[^2]. However, our Laurel resident is a bit of a player, and she has multiple people sliding into her (letter) DMs. 

When the letter is sent, its addressed to her address (building+room) but also to her. This is because inside her suite, there are multiple people, and all of them can receive their own mail so we need to specify who this mail is for. When she reads the address info, her suite members will look at the sender address, sender name, destination address, destination name, and in combination with those four items determine exactly what conversation this letter is a part of. Comparing this analogy to the transport layer, the address info is the IP address of the host, the people here are *processes* that are able to connect on the network, and the name of the people are the *port numbers*. 

> **Definition**: A **port number** is a $16$ bit, unsigned integer that provides an identification for a process running on a host that can connect to other processes over a network.

Port numbers are necessary because a host may have many processes running concurrently, and the host will need to find a way to match incoming mail to a process. Whenever mail comes in, the transport layer protocol being used will observe the $4$-tuple of `(src_ip, src_port, dst_ip, dst_port)` and pass it along to the proper process thread. We actually specify like this because $1$ port on a source or destination may have multiple simultaneous connections (consider a website that has multiple users at the same time). This combination of $4$ items is called a *socket address* and refers to a specific *socket*.

> **Definition**: A **socket** is a programming interface that allows for the sending/receiving of data for a specific process[^3][^4].

## User Datagram Protocol : AKA YOLO Protocol

The most straightforward protocol in the transport layer is called UDP, which is the yolo of the internet. This protocol is absurdly simple, and effectively does nothing other than send a packet. You have no idea whether or not your message has made it to the destination, or whether it was dropped along the way. With this in mind, the UDP packet structure is incredibly simple[^5][^6].

{{< figure library="true" src="computerNetworking/transport_page1_udpheader.png" title="UDP Packet" lightbox="true" >}}

We see that there is effectively nothing, we have the ports, the data length, a $16$ bit checksum, and then our data. The length field includes the size of the header as well, meaning that we are allowed to have $2^{16}-8$ bytes of data present inside of a UDP packet (often called segment).

Note that there is no IP address information in this packet. That is because the IP address is added when the network layer software creates the IP packet. All of this data inside the UDP packet goes inside the "data" part of the IP packet, so putting it all together it would look like a sequence of headers, followed by the data you actually care about!

Ok, so if this is the garbage method that is just a YOLO, what is the point of using it? Mostly because its so lightweight
1. No connection required, send the message and thats it. The server just receives some unsolicited mail and reads it
2. Minimal overhead allows for more data sent with fewer packets
3. Low overhead means developer is able to be flexible and add their own functionality in

## Python Socket Programming with UDP

Lets actually code up a simple text message program, so we can see UDP sockets working in action. In this, we will just have a simple message sending client, and a server that prints it out. We will also be using Python for the purposes of our code, so hopefully you know some python fundementals. 

### UDP Client

To begin, lets observe how we actually can send messages to each other over UDP. We will create a UDP Client class that takes a user string input and fires it out to some servers IP address and Port

```python3
import socket

serverName = "127.0.0.1"
serverPort = 12500

with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as clientSocket:
    while True:
        message = input()
        clientSocket.sendto(message.encode(), (serverName, serverPort))
```

Lets walk through what this is doing line by line. First we import the necessary library functions in `import socket`. Next we set variables to represent the IP address and port that we want to send our messages to

```python3
serverName = "127.0.0.1"
serverPort = 12500
```

Remember that `127.0.0.1` means "send this message to this device. Next we open our UDP socket that we would like to send messages through using
```python3
with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as clientSocket:
```
where you can note that the `with` keyword is the same one that you can use for a file stream, just enhancing the comparison. 
`socket.AF_INET` means that we are using IPv4 and `socket.SOCK_DGRAM` means that our transport protocol is UDP. If you'd like to use IPv6 then use `AF_INET6`. The `while` loop and `message=input()` should be self explanatory. 

Finally we send the message using
```python3
clientSocket.sendto(message.encode(), (serverName, serverPort))
```
The tuple of `(serverName, serverPort)` shows our socket who to send the packet to, and `message.encode()` converts our string into a binary sequence with a default encoding of UTF-8.

But wait, where do we set our own IP addresses and Ports? For this purpose it doesn't matter because we are sending the message. So we just let the library handle that shit for us so we don't have to worry about it.

### UDP Server

Now we will write the code to actually receive these messages that we are sending. Just as before I'll put the code first and then walk through it

```python3
import socket

serverPort = 12500

serverSocket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
serverSocket.bind(("", serverPort))
print("Starting Server")
try:
    while True:
        message, clientAddress = serverSocket.recvfrom(2048)
        print(message.decode(), clientAddress)
finally:
    serverSocket.close()
    print("Socket Closed")
```

[^1]: https://en.wikipedia.org/wiki/Two_Generals%27_Problem
[^2]: The fact that Laurel has no entrence/exit in the back is criminal
[^3]: https://stackoverflow.com/questions/16233193/what-exactly-is-socket
[^4]: From the stack exchange answer, its effectively a glorified file reader that reads from the network, specifically listening to messages on the connection between two processes.
[^5]: Packet image from https://en.wikipedia.org/wiki/User_Datagram_Protocol#UDP_datagram_structure
[^6]: Authoratative source can be found here for UDP [RFC 768](https://www.rfc-editor.org/rfc/rfc768)