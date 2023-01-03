---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "A Reduction-Based Strategy for Optimal Control of Bose-Einstein Condensates"
authors: ["jimmie-adriazola","admin"]
date: 2022-2-28T09:35:18-05:00
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: 2022-02-28T09:35:18-05:00

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: "Physical Review E"
publication_short: "Phys. Rev. E"

abstract: "Applications of Bose-Einstein Condensates (BEC) often require that the condensate be prepared in a specific complex state. Optimal control is a reliable framework to prepare such a state while avoiding undesirable excitations, and, when applied to the time-dependent Gross-Pitaevskii Equation (GPE) model of BEC in multiple space dimensions, results in a large computational problem. We propose a control method based on first reducing the problem, using a Galerkin expansion, from a PDE to a low-dimensional Hamiltonian ODE system. We then apply a two-stage hybrid control strategy. At the first stage, we approximate the control using a second Galerkin-like method known as CRAB to derive a finite-dimensional nonlinear programming problem, which we solve with a differential evolution (DE) algorithm. This search method then yields a candidate local minimum which we further refine using a variant of gradient descent. This hybrid strategy allows us to greatly reduce excitations both in the reduced model and the full GPE system."

# Summary. An optional shortened abstract.
summary: ""

tags: ["BEC"]
categories: ["published"]
featured: true

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
links:
 - name: Journal
   url: https://doi.org/10.1103/PhysRevE.105.025311
 - name: arXiv
   url: "https://arxiv.org/abs/2111.14266"
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

url_pdf:  https://doi.org/10.1103/PhysRevE.105.025311
url_code:
url_dataset:
url_poster:
url_project:
url_slides:
url_source:
url_video:

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: ""
---
