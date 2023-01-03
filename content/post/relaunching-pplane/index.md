---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Relaunching dfield & pplane"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2021-02-02T22:06:58-05:00
lastmod: 2021-02-02T22:06:58-05:00
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

{{% callout note %}}
I'm happy to report that my work here has been rendered obsolete. Brian Hong at the Mathworks has written new programs that are sleek and modern and in every way superior. They're even documented!  Download them [here](https://github.com/MathWorks-Teaching-Resources/Phase-Plane-and-Slope-Field).
{{% /callout %}}

I have written a new front end and made _many, many_ fixes to the old MATLAB programs **dfield** and **pplane**, that make them compatible with MATLAB Release 2020b. To distinguish these new working programs, I've renamed them:

**matdfield** draws direction fields for (possibly nonautonomous) first-order scalar ordinary differential equations.

**matpplane** draws phase plane portraits for autonomous first-order systems of two ordinary differential equations.

I've also created a simple launcher packaged as a *MATLAB App*, so all a student needs to do is click a button.

 The best place to download them is the [Mathworks File Exchange](https://www.mathworks.com/matlabcentral/fileexchange/86937-matdfpp).

They have lots of nice features: 

* drawing level sets
* finding equilibria and linearizing about them.
* finding stable & unstable manifolds
* saving user generated equation definitions to files and galleries.

These programs were originally copyrighted by John Polking between 1995 and 2003. The textbook *Ordinary Differential Equations Using MATLAB*, 3rd edition, contains a manual for the programs, but they are pretty self-explanatory.

The codes were last properly updated in 2003 for MATLAB 6.5. The MATLAB File Exchange contains many submissions that got the codes running again on some release or other, but none that systematically attempted to fix _al_l the broken components. 

I have tried to fix all the errors but have not managed to fix everything. I have examined every warning produced by the Code Compatibility Report and the Code Analyzer Report features in MATLAB. I have been able to fix almost all of these. Where I could not, I suppressed the warning and left a comment. Most of the remaining warnings are due to arrays that change size over iterations, and there's not much to do about them. Many came from an overuse of the `eval` and `feval` commands, which I have replaced by more modern coding constructs.

As far as I can tell everything works except for:

* The 'Revert' button 
* The 'Zoom Back' feature in pplane (works in dfield, should be able to fix in pplane).
* "Find a nearly closed orbit" in pplane.
* The message section at the bottom of the display is garbled in pplane (again, it works in dfield so should be fixable.)
* Certain forms of complicated ODE cause crashes.

Under the hood, the program is a mess. *There is a lot I would like to fix, but the structure of the original function would make modernization nearly impossible.* 

* Each program (pplane & dfield) uses a structure where each callback, instead of being a separate function, is given by a recursive call to the main program (matdfield) or (matpplane), determined by a very long `if-then-else` structure. I have tried breaking out the contents of individual `elseif` blocks into separate functions, in their own m-files. This has generally led to strange and hard-to-trace errors, so I abandoned the effort. This structure makes understanding the scope of variables very challenging.
* The function of each `elseif` block is rarely documented, although I added some documentation in my attempt to understand what each does.
* A lot of code is dedicated to fiddling with window dimensions and similar display issues. The variable names used to do this are not self-explanatory and contain lots of magic numbers.
* A lot of code is dedicated to string manipulation. Either this was written before MATLAB added straightforward string-handling features, or else the authors didn't know about these features. To be fair, _the original programs were written before Google existed(!)_, so discovering MATLAB functionality would have involved searching in books!
* There are a lot of calls to `eval` and `feval` which leads to hard-to-decipher codes. I have corrected this in the cases where such constructions confused the Code Analyzer.

Nonetheless, there are a lot of simple cleanups that might make this more maintainable, including lots of repeated code that should be moved to separate m-files and made into functions. I will continue to maintain this code and make sure that it remains compatible with future MATLAB releases. If anyone would like to help out, or finds an error they'd like to fix, the code will be available at https://github.com/manroygood. Any other bugs you find, please let me know.

**Acknowledgments**: The current codes are based on revisions by Nathanael Kazmierczak (pplane) and Giampiero Campa (dfield). I gratefully acknowledge their efforts and those of others who posted fixes MATLAB File Exchange. Several other people sent me partially working codes including Mac Hyman, Hil Meijer, Joceline Lega, Ross Parker, and Bjorn Sanstede. I got lots of help by posting questions to MATLAB Answers. These were usually answered (within minutes) by Walter Roberson, of the Mathworks. Div Tiwari, also of the Mathworks, provided additional useful suggestions.