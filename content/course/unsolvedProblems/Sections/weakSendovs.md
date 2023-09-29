---
title: Weak Sendov Conjecture
linktitle: Weak Sendov Conjecture
toc: true
type: book
date: 2023-01-16
draft: false
tags:
    - calculus
    - analysis
    - polynomials

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 5003
---

## Problem Description

Sendov's Conjecture is a problem involving polynomials and their roots, given we know information about the roots of a polynomial, are we garaunteed that we will be able to find a critical point of the polynomial close by?

> **Sendov's Conjecture**: Let $f:\mathbb{C}\rightarrow\mathbb{C}$ be a polynomial with $n\geq 2$ roots such that all roots $|r_j|\leq 1$. Let $c$ be a critical point of $f$, does there always exist some root $r_j$ such that $|c-r_j|\leq 1$.

What we are saying is asking the question if every critical point has a root of the polynomial of distance at most $1$ away from it. Note that the theorem is related to complex numbers. This theorem has been proven for $n<9$ and for $n$ sufficiently large, there are just some cases in between that need to be shown.

A simpler version can be shown as

> **Weak Sendov's Conjecture**: Let $f:\mathbb{R}\rightarrow\mathbb{R}$ be a polynomial with $n$ real roots where $|r_j|\leq 1$. For every critical point $c$ of $f$, does there exist a root $r_j$ such that $|c-r_j|\leq 1$