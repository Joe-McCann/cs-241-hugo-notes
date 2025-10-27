---
# Course title, summary, and position.
title: The Transport Layer
linktitle: Transport Layer
summary: This I Promise You
weight: 400

# Page metadata.
date: "2025-10-26T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: book  # Do not modify.

---

__William Joe McCann__

## Finally We Can Get Somewhere

In the previous chapters, we were looking at the network from such a low, abstract level, that it was incredibly detached from the day-to-day internet experience that you are likely accustomed to. As my mother always says "never ask how the sausage is made". While it is important to know how everything works under the hood[^1], we will now be moving into the first set of material that you may very well interact with in your day job as a software engineer.

In the transport layer, we now will consider the underlying network to be some sort of abstract pipe that can kill our message at any moment in time. With this in mind we need to come up with solutions around how we can take this unreliable pipe, and actually use it in a meaningful way. 

[^1]: Remember from prior the Law of Leaky Abstractions