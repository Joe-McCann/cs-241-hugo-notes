---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Transfer entropy for network reconstruction in a simple dynamical model"
event: "NJIT Applied Mathematics Colloquium"
event_url: "https://math.njit.edu/applied-math-colloquium-spring-2020-0"
location: New Jersey Institute of Technology
address:
  street:
  city: Newark
  region: NJ
  postcode:
  country:
summary: "My 'what I did on my sabbatical' talk."
abstract: "Dynamics on a network are ubiquitous in areas as diverse as genetics, meteorology, and the social sciences. By a network, we simply mean a system of interacting agents. A common problem is to reconstruct a network given the behavior of its agents, that is, given a set of time series describing the states of each node on the network, determine which other nodes are influencing that node. Information theory provides one set of tools for doing so in a model-independent manner. A popular tool in this game is called transfer entropy. Transfer entropy measures the reduction in uncertainty in predicting the state of one agent at a time (t+1) from its state at time t given the additional knowledge of the state of a second j at time t, compared with predicting it the knowledge of the first agent alone. This can be thought of as measuring the transfer of information from the second agent to the first. We analyze a simple model in which we derive asymptotic formulae relating the transfer entropy between two nodes to the strength of the connection between the nodes. Higher-order terms in this formula show the subtle effect of network topology on the transfer entropy. This allows for more accurate network reconstruction in this particular model and points to possible sources of errors when transfer entropy is used to reconstruct networks in a more general setting."

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
date: 2020-02-14T11:40:00-05:00
date_end: 2020-02-14T12:40:00-05:00
all_day: false

# Schedule page publish date (NOT talk date).
publishDate: 2020-02-15T16:09:48-05:00

authors: ["admin"]
tags: []

# Is this a featured talk? (true/false)
featured: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

# Optional filename of your slides within your talk's folder or a URL.
url_slides: slides.pdf
url_code:
url_pdf:
url_video:

# Markdown Slides (optional).
#   Associate this talk with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
