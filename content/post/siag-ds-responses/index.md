---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Summary of Responses to my post to the SIAG-DS mailing list"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2021-02-02T22:05:43-05:00
lastmod: 2021-02-02T22:05:43-05:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

## The email

On January 5 I sent [this post](http://lists.siam.org/pipermail/siam-dy/2021-January/000776.html) to the SIAG-DS mailing list about the old MATLAB programs dfield and pplane. I posted the same question to the Facebook group *STEM faculty blundering through remote teaching in a pandemic*. My goal was to see if I could get the programs running again to use in my undergrad ODE classes, and, in the longer term, to rewrite it from scratch. 

## The responses

Over the next two weeks I received about 30 response emails. I learned a lot from these emails and I thought the best thing I could do is share them. 

The main thing I learned is that a lot of people still like these programs and would like to be able to use them. A second thing I learned is that lots of people have managed to fix just enough bugs to keep the program hobbling along. A few people have uploaded these partial fixes but nobody, as far as I was able to tell, had made an attempt to systematically go through the code and fix everything. One person who had made some fixes, and had the forethought to take notes on it as he went, is Nathanael Kazmierczak, now a grad student at Caltech. He sent me his updates and his notes, and patiently answered my questions via email. I decided to run these code, "push all the buttons," and try to fix all the errors. I believe I have done this. Also, *I hereby appoint myself maintainer of these codes for the near future or until we can get something better in MATLAB.*

## My requirements

Lots of people wrote me to tell me about other options for graphing direction fields and phase planes and more. I knew about a lot of these and have even used a few of them myself for research and teaching. None, in my opinion suit my particular needs the way as well as dfield and pplane. These are:

1. **It should be in MATLAB.** Our undergrads use MATLAB throughout the curriculum. Using dfield and pplane allows me to assign as the first assignment: _get MATLAB running and run this pre-written program,_ without asking them to students to learn any programming.
2. **It should be really really simple** Our 200-level ODE course typically runs 10 sections and 300 students per semester, many taught by adjuncts and lecturers, who aren't necessarily experts in programming. Many of the students have just transferred from two-year colleges that don't use MATLAB as much as we do. I just want it to work.
3. **It should have a GUI**.

## Other options

I'll arrange these by suitable audience and purpose:

### Dfield/pplane equivalents

Good for teaching undergraduates

* [Slopes App](https://sites.google.com/a/pepperdine.edu/slopes/) This is a terrific app for iOS and Android devices, being written by a group of students and faculty at Pepperdine University led by Timothy Lucas. It is great to use on an iPad during remote lectures and the graphics are stunning compared to pplane and dfield. The Android version is new and not-yet full-featured.  I don't think that robust numerical libraries for these devices exist, so they have had to write a lot of the numerical guts of the code themselves. As such, I have managed to break the code on several occasions while trying out exercises and examples I wanted to use. Tim has been very responsive to my inquiries and is aware of the issues.
* [Java implementation of dfield/pplane](https://www.cs.unm.edu/~joel/dfield/) Authorized clone of the original dfield/pplane programs. Requires a Java Runtime Engine. Works fine, bad graphics export.
* [PyPlane](https://github.com/m-squared96/PyPLANE) An open-source reimplementation of pplane and dfield written in Python. It appears to be under active development but to run it on Mac or Windows, you need to clone a GitHub repo, so that's too hard for a big undergraduate class. Someday there may be binaries...
* [Javascript Implementation of dfield and pplane](https://homepages.bluffton.edu/~nesterd/apps/slopefields.html) On the webpage of Darryl Nester of Bluffton University, h/t [Sean Fitzpatrick](https://www.cs.uleth.ca/~fitzpat/)
* [Direction Field Explorer](https://c3d.libretexts.org/DirectionField/index.html) From the LibreTexts free textbook website. h/t [Sean Fitzpatrick](https://www.cs.uleth.ca/~fitzpat/)
* [phaseR](https://github.com/mjg211/phaseR) an R package for exploring one and two dimensional autonomous ODE systems, by Michael Grayling et al. Not a GUI, so not for an intro class. h/t [John Guckenheimer](https://pi.math.cornell.edu/~gucken/).

### Writing your own

* [Matthew Mizuhara](https://owd.tcnj.edu/~mizuharm/) has a basic equivalent of dfield on his [GitHub page](https://github.com/mizuharm/Code-for-teaching)
* [Harry Dankowicz](http://danko.mechanical.illinois.edu/index.htm) shared some codes with me. I didn't get his permission to share them, but what impressed me was that the documentation was many times longer than the codes themselves. I commented on how thoughtfully done this was and he responded, "As to documentation, this is something I place a premium on, also in my development of more sophisticated tools. I have often found that it actually helps tighten up the software implementation as well, especially when a mathematical notation and a logical flow can be developed in parallel with the encoding and both made to mirror one another." 
* [Bjorn Sandstede](http://bjornsandstede.com) is using [Jupyter notebooks](https://github.com/sandstede-lab-teaching) for a similar purpose, with what sounds like significant technical support from Brown University.
* Beyond heavy reliance on the Slopes app, I made extensive use of MATLAB Live Scripts this last time that I taught out of Strogatz's book. I wanted to show how, with today's tools, it is straightforward to reproduce all the classical results Strogatz discusses and introduce students to the interplay between theoretical and numerical studies that leads to insights. I ended up writing something of a [MATLAB Companion to Strogatz]({{< relref "../../course/math473/#companion-to-the-textbook " >}}).

### More Advanced Students and Research

I like these but they don't fit the bill for big intro classes. This is just the ideas people sent me, for a more complete list, look at the [Dynamical Systems Software list](https://dsweb.siam.org/Software) at DSWeb, h/t Andrew Bernoff.

* [matcont](https://sourceforge.net/projects/matcont/) MATLAB package for continuation and bifurcation in discrete and continuous systems. I have used this in my own research and in a graduate class. Highly recommended
* [Julia](julialang.org) I've been meaning to learn Julia for years! Somehow it always seems easier to stick with MATLAB. In particular, Isaia Nisoli pointed out [this idea](https://discourse.julialang.org/t/plotting-a-phase-portrait-of-a-differential-equation/29208/8).
* [Chebfun](chebfun.org) from Nick Trefethen and company in Oxford. 
* [PyDSTool](https://pydstool.github.io) is a nice program that is no longer in active development, h/t John Tyson
* [Brain Dynamics Toolbox]([https://bdtoolbox.org](https://bdtoolbox.org/)), from Stuart Heitmann, is an open MATLAB toolbox for simulating dynamical systems and plotting phase planes etc. It was designed for computational neuroscience but can be applied to dynamical systems in any domain (ODEs, SDEs, DDEs, PDEs). Students can implement their own models (often less than 100 lines of code) and run them in the graphical interface without any graphical programming effort. The latest version uses the modern MATLAB uitools graphical interface. It requires matlab R2019b or newer.
* [XPP](http://www.math.pitt.edu/~bard/bardware/xpp/xpp.html) from Bard Ermentrout, remains very popular.