---
title: Robin's Inequality 🐦
linktitle: Robin's Inequality 🐦
toc: true
type: book
date: 2023-09-27
draft: false
tags:
    - number theory
    - prime numbers

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5005
---

## Problem Description

This problem is one that I have tinkered around with a lot, and work that I am actually quite proud of. Sadly this problem is an equivalent formulation as the Riemann Hypothesis, so I will likely never actually prove it, however theres some cool progress that I can make on manual computations.

Before we can actually formulate Robin's Inequality, we need to define ourselves a function $\sigma$ where $\sigma(n)$ is the *sum of divisors function*.

> **Definition**: Let $\sigma(n):\mathbb{N}\rightarrow\mathbb{N}$ be the sum of all positive integers $k < n$ such that $n$ is divisible by $k$