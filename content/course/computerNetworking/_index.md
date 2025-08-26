---
# Course title, summary, and position.
title: Computer Networking
linktitle: Computer Networking
summary: How does the internet work?
weight: 25

# Page metadata.
date: "2025-06-8T00:00:00Z"
lastmod: "2025-06-8T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: book  # Do not modify.
tags: 
- software engineering
- cs356

---

__William Joe McCann__

### How We Communicate with Each Other

In this material, we will cover the fundementals of computer networking, with a specific emphasis on its application with regards to the Internet (which is surprisingly[^1] not the only computer network). These fundementals were created many times out of necessity rather than strict planning: a problem was encountered by people who needed to perform a certain task, and solutions were created for said problems. 

In this way we can think of the development of computer networks as this chaotic process in which engineers and researchers just did things to fit their needs, and other people took hold of those creations to fit their needs. What we find is this insane patchwork of many different protocols, technologies, and architectures for various purposes. Those, of course, just being the ones we know of too, when in reality there are likely just as many secret solutions being used by engineers in companies all across the world!

In this class, we will go over this problem recognizing and solving process, so that you will be able to apply what you learn when you encounter a networking problem in the future. This material will be heavily based on the book **Computer Networking: A Top Down Approach** by Kurose and Ross[^2]. Free resources from this book can be found [here](https://gaia.cs.umass.edu/kurose_ross/index.php)

---

### Terminology

In this page, we will also be listing off different pieces of notation and terminology so that you have a singular reference in case you get confused.

#### Data Sizes

A **byte** is $8$ **bits**. If you see a lowercase $b$, then that means the unit is in bits, if it is an uppercase B then that means the unit is in bytes. If you see units like *kilo*, *mega*, *giga*, etc this is the standard metric units that go in powers of $10$. So *kilo* is $10^3$, *mega* is $10^6$, and so forth. There was a time that people used powers of $2$, but this is **wrong**[^3]

[^1]: Surprising as in its the only one most people knowingly interact with.
[^2]: [ISBN-10: 9356061319](https://www.amazon.com/Computer-Networking-Top-Down-James-Kurose/dp/9356061319)
[^3]: According to [US Law](https://en.wikipedia.org/wiki/Gigabyte#Consumer_confusion) and standards bodies
