---
title: GTFO
linktitle: An Intro to Git and Github
toc: true
type: book
date: 2023-01-16
draft: false
tags:
    - cs241
    - software engineering
    - git

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 205
---

## An Intro to Git

Most people, in some capacity, have heard of `git`, or at least one of the many, many services that utilize `git`. Some of these services are such staples in the industry that I know that I personally didn't know that `git` was entirely seperate than Github the website! As such, I will teach you `git` first before mentioning some pieces of Github, all with the intention of making you able to contribute to open source projects.

Before continuing make sure you install `git` from [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

In order to see if its installed run

```bash
git --version
```

in your terminal, and get something like

```bash
$ git --version
git version 2.25.1
```

as the proper output. This will be with whatever version you install. Note that if you get that `git` is not recognized you may need to add it to your Path.

### What is Git?
