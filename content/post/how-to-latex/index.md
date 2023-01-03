---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "How should we LaTeX?"
subtitle: “A style guide for my students and long-suffering collaborators."
summary: ""
authors: [“admin"]
tags: []
categories: []
date: 2021-07-02T22:22:55-04:00
lastmod: 2021-07-02T22:22:55-04:00
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

{{% callout warning %}} When I wrote this post, I had just learned about the _physics_ package for $\LaTeX$ which can make typesetting a lot of the things I work on a lot easier.  I've since found out from [this discussion](https://tex.stackexchange.com/questions/470819/macros-dv-and-pdv-eat-the-subsequent-parenthesis-argument) that this package can be somewhat dangerous. I'm not giving up with it, but handle with care!{{% /callout %}} 

For students first learning $\LaTeX$, it can be intimidating, but never fear, this document will help. If we’re going to publish something together, it will eventually have to meet my standards. By following this document, you’ll learn my approach to writing in $\LaTeX$ and hopefully avoid having to do things twice.

Some ideas from this document were taken from other documents I found on the web such as [this one](https://web.stanford.edu/class/ee364b/latex_templates/template_notes.pdf) from Stephen Boyd at Stanford, [this one](https://inverseprobability.com/latexStyleGuide) from Neil Lawrence at Cambridge, and [this one](https://web.science.mq.edu.au/~rdale/resources/writingnotes/latexstyle.html) from Robert Dale at Macquarie. I started writing this thinking it was a guide to help students do things my way, but in the process of getting my thoughts down, I had to look things up. I learned a few things that I will be adopting in my own writing.

## Basic principles

In my mind, there are two basic principles in writing good $\LaTeX$. 

* Separate meaning from formatting
* Use packages and class files written by other people.

These two principles are related.

### Separating meaning from formatting

My limited of the understanding of the history is that Knuth created $\TeX$ to be able to enable computers to typeset mathematics and to do so consistently regardless of the computer being used. Lamport then built $\LaTeX$ on top of Knuth's foundation in order to _separate presentation from content._ A primary example is given by the way fractions are represented in the two systems. In $\TeX$, the fraction $\frac12$ is written as `${1}\over{2}$`. That is a typesetting command: it states that the numeral "one" is placed "over" the numeral "two." While any $\TeX$ code is legal in $\LaTeX$, in the latter system the preferred form is `$\frac{1}{2}$` which is easier for a human reader to interpret as a fraction. 

Well-constructed $\LaTeX$ should be easy  to read, and should be focused on representing meaning, not simply formatting. 

### Macros

Good $\LaTeX$ writing should make extensive use of macros. In writing computer programs, whenever there is a piece of code that gets re-used multiple times and accomplishes a specific task, it makes sense to define a function and to call that function each time you need to accomplish that task. In writing $\LaTeX$, the equivalent idea is to use macros to replace frequently used snippets. There are a number of important reasons for this (how we do so will be discussed later):

* Macros help you to encode meaning rather than typesetting instructions. For example the absolute value of $x$ should appear as `$\abs{x}$`, not as `$|x|$`.  (In this case, rather than write your own, you should use the macro provided by the *physics* package.)
* Long strings of typesetting are hard for you to parse and to type exactly the same every time. Suppose you define a complicated mathematical object that you decide to name ${{\mathcal C}^\infty_{\rm{c}}}$. Do you want to type `{\mathcal C}^\infty_{\rm{c}}` every time you use it and have to have that expression repeatedly taking up half a line of your .tex document? Or would you rather define a macro `\CIc` that you can use repeatedly?
* Macros make it easier to change the document. Suppose that you decide instead to change your notation to ${{\mathfrak C}^\infty_{\rm{c}}}$. If you define a macro, then changing the macro once and your change will appear everywhere. Otherwise, you have to carefully comb through your document to find every occurrence of this expression.

In general, if you are trying to typeset a common mathematical function, such as a derivative, norm, or absolute value, *you should find a package that contains the macro you need.* If, on the other hand, your paper contains a vector-valued variable named $\mathbf{p}$, you should define a macro, rather than typing `\mathbf{p}` dozens of times.

I have used four methods to declare macros:

* `newcommand`  Can contain arguments examples
  * `\newcommand{\p}{\mathbf{p}}` Now you can type `$\p$` to get $\mathbf{p}$.
  * `\newcommand{\CIc}{{\mathcal C}^\infty_{\rm{c}}}`for the above example
  * `\newcommand{\diff}[2]{\frac{d #1}{d #2}}` defines a basic derivative macro  Example: `\diff{y}{x}` would give $\frac{dy}{dx}$. (Better: use the **physics** package.)
* `renewcommand` is just like `newcommand` but you use if the command you want to assign is already assigned to something else.
* `DeclareMathOperator` (requires the ams math package---note the capitalization). You'll note that functions such as sine and cosine are written in Roman characters and are defined as operators, which effects the spacing between them and their argument. For example the standard way to write $\sin{\theta}$ is `\sin{\theta}`. However there's no hyperbolic secant built in so you could define  `\DeclareMathOperator{\sech}` which you'd call using `\sech{x}`. (Better: use the **physics** package which does some neat tricks with parentheses.)
* `DeclarePairedDelimiters` Here is the evolution of my approach to absolute values
  1. Basic: `|x|`
  2. Better: `\left| x \right|` is better because the absolute value bars scale to the height of the content between them.
  3. Better  `\left\lvert x \right\rvert` . The delimiters`\lvert` and `rvert` are preferred over the bars. But now we've gotten deep into writing formatting so
  4. Better: define a macro `\newcommand{\abs}[1]{\left\lvert #1 \right \rvert}` and call `\abs{x}`
  5. Better: there are many similar constructions involving such *paired delimiters*. The **mathtools** package provides commands to handle these automatically. For example, we can define our macro as `\DeclarePairedDelimiter{\abs}{\lvert}{\rvert}` , which we would again call as `\abs{x}`. There are other constructions where you may want two objects inside a pair of delimiters to denote some sort of product. The **mathtools** package can do this too, such as `\DeclarePairedDelimiterX{\innerp}[2]{\langle}{\rangle}{#1,#2}` to define an inner product.  Only use this construct if you can't find a pre-defined macro in the **physics** package; see the next point.
  6. Best: use the **physics** package which defines several macros for common expressions involving paired delimiters. To make the height of the delimiters adjusted to enclose the contents,  call them with an asterisk as in `\abs*{\frac{p(x)}{q(x)}}` to get $\left\lvert \frac{p(x)}{q(x)} \right\rvert$.
* When might you actually use the paired delimiter syntax? In my case, the **physics** package does not define an inner product (except in bra- and -ket notation). However the example above: `\DeclarePairedDelimiterX{\innerp}[2]{\langle}{\rangle}{#1,#2}` does it nicely so that `\innerp*{\frac{p}{q}}{f}` is rendered $\left\langle \frac{p}{q},f \right\rangle$. Bonus tip: use `\langle` and `\rangle` not greater than or less than signs for inner products.

## Using packages

Here are the packages that are called in my template. In some cases, the order they're called matters, as package B may depend on package A. Each is fully documented, so I'll just say why I like it and the major ways I use it. 

#### Fixing outdated things about $\LaTeX$

* **csquotes** is a sophisticated package for handling quotes. I use it to do one thing. In order to get nice curly quotation marks like "Happy birthday!" you have to write $\mathtt{``Happy\  Birthday!''}$  with two single left-hand quotes on the left and two single right-hand quotes on the right. The package allows you to use plain old straight quotes like $\mathtt{"Happy\ Birthday!"}$ and still get curly quotes.

  ```latex 
  \usepackage{csquotes}
  \MakeOuterQuote{"}  
  ```

* **inputenc** allows you to use modern character encodings. When $\LaTeX$ was written, computers had access to a much smaller character set and could not natively display accented characters like é, ñ, ö, so that you had to enter these as `\'{e}` , `\~{n}`, and `\"{o}`. A more modern character-encoding standard is *utf-8*. More modern engines such as XeTeX and LuaTeX​ can handle utf8 natively, but the more common pdftex cannot. The **inputenc** package allows you to use accented characters, which makes it easier to write Schrödinger and Poincaré. If you create your document in Overleaf, the file will automatically be encoded as utf8. If you create the document in TeXShop, then you can set the preferences to use utf8 encoding. If you want to use utf8 encoding in an existing document in TeXShop, add a line `% !TEX encoding = UTF-8 Unicode` at the top of the main .tex file. To load this package use:

  ```latex
  \usepackage[utf8]{inputenc}
  ```

* **[amsmath, amsthm, amssymb](https://texdoc.org/serve/short-math-guide/0)** These are packages from the American Mathematical Society that have lots of features that make writing math easier and better: from their names, you can guess that they introduce (i) general math stuff (ii) environments for defining theorems and (ii) additional math symbols. It'w worth reading the documentation to learn all the features. When I was writing my dissertation, the postdoc in the next office told me to stop using `\eqnarray` and to learn about the the equation alignment features. He was right. Other important features:

  * Use the AMS defined `\eqref` command to reference equations rather than the basic `\ref`. This puts parentheses around each reference.
  * `\DeclareMathOperator` as described above
  * a `\cases` environment
  * the `\pmatrix` and `\bmatrix` environments are the standard way to define matrices.

* **mathtools** builds on top of the AMS packages. It provides the **pairedDelimiters** construct I used to build the inner product macro. Another useful command: regular $\LaTeX$ renders the command `:=` with poor vertical alignment. Mathtools provides `coloneqq` that fixes this.

* **physics** This package requires amsmath and provides lots of things for mathematical physics and differential equations.

  * Its commands `\dv,` `\pdv` , and `\fdv` are the best way I've seen to typeset derivatives, partial derivatives, and functional derivatives. Multiple and mixed derivatives also handled elegantly

  * Robust and convenient macros for norms, absolute values, Poisson brackets, etc.

  * No missing trig functions. Redefine the trigonometric functions so that parentheses are defined automatically include parentheses, i.e., `\sin(x)` will render as $\sin{(x)}$ whereas in regular $\LaTeX$, you'd need to type `\sin{(x)}`.

  * The `\qqtext` macro eliminates the need for this construction to put text and space in the middle of math. For example to get 
    $$
    a = b \quad \text{and} \quad c=d
    $$
    we'd usually have to write `a = b \quad \text{and} \quad c=d`. But the physics package reduces this to `a = b \qqtext{and} c=d`. Even better, the package defines a handful of further macros for common words like *and* so this can be further reduced to `a = b \qqand c=d`.

* **natbib** is a package for working with BibTeX. In particular calling the command as 

  ```latex
  \usepackage[numbers,sort&compress]{natbib}  
  ```

  turns a citation that would have rendered as $[6,1,2]$ into $[1,2,6]$, and turns $[1,2,3,4]$ into $[1-4]$. Saves space and looks better!

* **breqn** allows matched delimiters such as `\left(` and `\right(` to match across line breaks.

* **fullpage** reduces margins so that you can see what you're writing

* **todonotes** does two things: (1) allows you to create to-do items and list them at the front of the document and (2) creates placeholders for figures that aren't ready yet.

* **showlabels** shows the labels next to equations and figures so it's easier to recall their names when you want to reference them.

## My style guide

Some reasons to have a style guide are for consistency and to conform to professional conventions. On top of that, poor style can be distracting to the reader. I learned a bunch of things while preparing this document, so this style guide represents how I intend to use $\LaTeX$ in the future, rather than how I have always used it. 

#### Overleaf

Do not email documents back and forth. Do not create a sequence of documents named `document1.tex`, `document2.tex`, etc. This can only lead to mistakes. Online tools such as [Overleaf](overleaf.com) handle shared documents much more elegantly. 

If you prefer to edit locally, then set up Dropbox integration under Account $\rightarrow$​ Account settings on Overleaf. Edit on your personal device. Syncing isn't instantaneous, but prevents versioning Hell. Sadly, this requires a premium or institutional Overleaf account.

#### Guidelines

* This is a hard one. `$...$` and `$$...$$` are plain $\TeX$. The $\LaTeX$ equivalents are `\( \)` and `\[ \]`. Does this make a difference? I don't know, but it's [recommended as best practice](https://tex.stackexchange.com/questions/503/why-is-preferable-to) and is supposed to improve spacing before and after equations, so I'm going to try to do it from now on. You can download a python script called [dedollar](https://mjsharpe.github.io/tex-software/) that will convert this for you. 

* Vectors: bold, usually lowercase. Don't use arrows. The best option is `\vb` (`\vb*` for Greek letters) from the **physics** package.

* Matrices: Uppercase, bold.

* Derivatives: Use the `\dv` command from the physics package, `\pdv` for partials `\fdv` for functional or Frechêt derivatives.

* The $\mathrm{d}$​ in an integral is typeset using the `\dd{x}` command from the **physics** package. This will automatically handle the spacing before the letter.

* Bibliograpy & References:
  * Use BibTeX. This is one area where I have especially strong opinions. Specific guidelines:
    * BibTeX is reusable between documents. Reuse prevents new mistakes. Try to download the BibTeX from the journal's website for an article rather than entering by hand, e.g., click on the "download citation" button [here](https://journals.aps.org/prfluids/abstract/10.1103/PhysRevFluids.4.124703).
    * While a BibTeX file is plain text, it's better not to have to edit it directly. Even if you use Overleaf, I find it preferable to edit on my own computer using the BibDesk app. That way I don't mess up all the brackets.
    * Use the appropriate document type.
    * Make sure that capitalized words in article titles ares surrounded by {}. Otherwise they'll get converted to lowercase.
    * Author names should be consistent. First and middle names should be shortened to initials. You don't have to actually edit the BibTeX file. Choose a bibliography style that automatically abbreviates for you, such as `\bibliographystlye{abbrv}`.
    * Leave the "number" field and the "month" field blank.
  * Use `\eqref` to reference equations. Use a a nonbreaking space (denoted with a `~`) before reference commands `\ref`, and `\cite`. For example `A graph of Eq.~\eqref{equation1} is shown in Fig.~\ref{thisfigure}`.
  * The words Figure and Equation should always be abbreviated in references. As above, the word "Fig." should always appear before a figure reference. Referenced equation numbers should always be proceeded by a word: usually "Eq." but sometimes it's more natural to say something like "Inequality (5)" or "Hamiltonian (6)".
  
* If a word appears in a subscript or superscript, it should be in Roman script. $X_{\rm{max}}$ not $X_{max}$, written as `$X_{\rm{max}}$`. Make a macro `\Xmax` and call that.

* Keep yourself out of it. Try to keep the use of first person to a minimum. In very rare circumstances, using first person is the most elegant way to avoid passive voice.

* Equations are part of sentences and need to be punctuated.

* Don't start a sentence with a symbol. Say "The norm $\lVert x\rVert$​ is finite,", not "$\lVert x\rVert$​​ is finite,"

* Make minimal use of hand-formatted spacing. Try to avoid using `\`, `\,`, etc. 

* Figures: 
  * If making figures in MATLAB, Use my startup.m file to get better defaults, in particular thicker lines, larger text in labels, use the $\LaTeX$​​ interpreter in all text. Thin lines disappear, especially when projected as slides.
  
  * Don't ever use the MATLAB green represented by `'g'`. Especially not in talks. It shows up very poorly on the screen. 
  
  * Align subfigures and avoid too much whitespace between and around them.
  
  * Don't get me started on colormaps! The old default MATLAB colormap, called **jet,** used in commands like `pcolor` and `surf ` is *bad, bad, bad* and should never be used. The new default colormap **parula** is better but still not optimal. A lot of perceptual research has gone into the design of the colormaps for the Python library **matplotlib** and has been ported over to [this MATLAB package](https://www.mathworks.com/matlabcentral/fileexchange/62729-matplotlib-perceptually-uniform-colormaps). The link contains a video with an explanation.
  
  * The ["Obsolete Packages and Commnds"](https://ctan.org/pkg/l2tabu-english) document recommends against using the `\begin{center}...\end{center}` syntax in a figure and instead use `centering` , i.e.
  
    ```latex
    \begin{figure}
    \centering
    \includegraphics{imagename}
    \end{figure}
    ```
  
    is preferred over
  
    ```latex
    \begin{figure}
    \begin{center}
    \includegraphics{imagename}
    \end{center}
    \end{figure}
    ```

## Getting help

* In TeXShop on a Mac, under help, click on "Show help on package" and enter the name of a package to read more about it.
* If you are running $\LaTeX$ on a Mac, you most likely have a program installed called *TeX Live Utility*. It has three tabs. The first allows you to keep your $\TeX$ installation up to date. It usually takes a minute to load while it checks for all the updates. The second tab "Packages" contains a list of all the installed packages. You can search for useful installed packages. For example, search for "Exam" and you'll find the Exam package, which is the right way to write an exam. (I have never clicked on the third tab "Backups")
* Google "How do I ... in Latex?" This will usually take you to a discussion on the [TeX StackExchange](https://tex.stackexchange.com/). Somebody has already asked the thing you want to know.

## My template and abbreviations files

At [this link](https://github.com/manroygood/LaTeX-Template) there are three $\LaTeX$ files: a master template file and two include files, one containing all the packages I usand the other containing the macros. I have also included a **startup.m** file for MATLAB. It should go in the search path and will run automatically when MATLAB starts. This changes the behavior of  MATLAB's text annotation functions such as `xlabel` and `title`. Any math that appears in such a command must now be enclosed in dollar signs, such as in `xlabel('$\omega$)`. This allows for both better looking text in your figures and opens up the possibility of much more complex math in the labels.
