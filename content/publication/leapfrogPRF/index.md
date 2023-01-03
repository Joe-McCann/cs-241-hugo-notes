---
title: "Stability of Leapfrogging Vortex Pairs: A Semi-analytic Approach"
date: 2019-12-26
publishDate: 2019-12-26
authors: ["brandon", "admin"]
publication_types: ["2"]
abstract: "We investigate the stability of a one-parameter family of periodic solutions of the four-vortex problem known as _leapfrogging_ orbits. These solutions, which consists of two pairs of identical yet oppositely-signed vortices, were known to W. Gröbli (1877) and  A. E. H. Love (1883), and can be parameterized by a dimensionless parameter $\\alpha$  related to the geometry of the initial configuration. Simulations by Acheson (2000) and numerical Floquet analysis by Tophøj and Aref (2012) both indicate, to many digits, that the bifurcation occurs when $\\alpha=\\phi^{-2}$, where $\\phi$ is the golden ratio. This study aims to explain the origin of this remarkable value. Using a trick from the gravitational two-body problem, we change variables to render the Floquet problem in an explicit form that is more amenable to analysis. We then implement G. W. Hill's method of harmonic balance to high order using computer algebra to construct a rapidly-converging sequence of asymptotic approximations to the bifurcation value, confirming the value found earlier."
featured: false
categories: ["published"]
tags: ["Vortices","Bifurcation"]
publication: " *Physical Review Fluids*"
url_pdf: ""
links:
  - name: Journal
    url: "https://link.aps.org/doi/10.1103/PhysRevFluids.4.124703"
  - name: arXiv
    url: "https://arxiv.org/abs/1908.08618"
---

### Erratum
The matrix in Eq. (19) should read 
$$\small{\left.A_{h}(\theta)\right\rvert_{h=\frac18}=
\begin{pmatrix}
 -\frac{4 \sin {2\theta}}{\sqrt{8 \cos {2\theta}+17}} & \frac{8 \cos ^2{2\theta}-12 \cos {2\theta}+3 \sqrt{8 \cos {2\theta}+17}-11}{2 (1-\cos
   {2\theta}) \sqrt{8 \cos {2\theta}+17}} \\\\
 \frac{-8 \cos^2{2\theta}-4 \cos {2\theta}-\sqrt{8 \cos {2\theta}+17}+7}{2 (\cos {2\theta}+1) \sqrt{8 \cos {2\theta}+17}} & \frac{4 \sin{2
   \theta}}{\sqrt{8 \cos {2\theta}+17}} 
\end{pmatrix}
}
$$
This is an isolated error in the writing and does not effect any of the mathematics in the paper.

### Addendum

In the published paper, we show that if the differential equation
$$
\frac{{\rm d}}{{\rm d}\theta}
\begin{pmatrix} \xi_- \\\\ \eta_+ \end{pmatrix}
= A_h(\theta)
\begin{pmatrix} \xi_- \\\\ \eta_+ \end{pmatrix}
$$
has a periodic orbit at $h=\frac18$ of period $\pi$, then the leapfrogging orbit bifurcates at $h=\frac18$. We tried using Mathematica to solve this equation exactly but it failed to return a closed form solution. Therefore in the paper we used high-precision numerics and a high-order perturbation expansion to give strong evidence that the solution with initial condition $(1,0)$ is indeed periodic.

In the summer of 2020 we posted a question to the website MathOverflow asking if anyone could find an exact solution to this equation. Almost immediately, we received the solution from Robert Israel, who found the answer using Maple:
$$
\begin{aligned}
    \xi_- (\theta)&=
        \phantom{-}\frac{1}{20}\left(
        1+4 \cos 2\theta+3\sqrt{17+8 \cos 2\theta}
        \right) \\\\
    \eta_+(\theta)&=
        -\frac{\tan\theta}{20}\left(
        1+4 \cos 2\theta+\sqrt{17+8 \cos 2\theta}
        \right) .
\end{aligned}
$$
Thus, without recourse to perturbation theory or numerics, this exact solution proves the theorem.


