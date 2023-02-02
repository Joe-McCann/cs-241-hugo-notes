---
title: Proving Set Equality
linktitle: How to Prove Two Sets Are Equal
toc: true
type: book
date: 2023-01-31
draft: false
tags:
    - cs241
    - set theory
    - set operations

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 305
---

## You Can Go Your Own Way 🎵 🎶

In this page I will explain one way of proving that two sets or set identities are equal. Obviously, there is never exclusively one way to do these things, so if you (for example) devide to learn algebraic identities for sets, then you are totally free to do so. This is just the way I learned it and I find it easy to understand.

### Worked Example

> **Theorem** (De Morgan's Laws): Let $A,B$ be sets. $$\bar{(A\cup B)}=\bar{A}\cap\bar{B}$$

The purpose of this section is to walk through proving this law, and then providing other examples that you can try. Remember that we said from the [theorems on set equality](/course/serTheory/sections/proofbycontradiction#no_largest_number_theorem)
