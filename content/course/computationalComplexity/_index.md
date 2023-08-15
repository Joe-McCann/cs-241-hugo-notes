---
# Course title, summary, and position.
title: Computational Complexity
linktitle: Computational Complexity
summary: How slow can you go?
weight: 10

# Page metadata.
date: "2021-07-06T00:00:00Z"
lastmod: "2022-07-06T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: book  # Do not modify.
tags: 
- software engineering
- cs241
- asymptotics
- computational complexity

---

__William Joe McCann__

### The Most Misunderstood Concept

When the discussion of computational complexity comes up, its often used as a way to compare algorithms in order to choose which is best. The standard discussion though goes as *"Unga bunga, my algo is $n$ and yours is $n^2$ so lets use mine"*, when normally it is not that simple. This stems from the fact that the notation was developed for number theory, and then co-opted by computer scientists who tend to be more loosey goosey with things[^1]. In this section of notes I will teach you what the notation *actually* means, which can give you a better insight into determining what this does. 

[^1]: In industry that is
