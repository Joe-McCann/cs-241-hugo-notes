---
# Course title, summary, and position.
title: The ELO Rating System ♟️
linktitle: The ELO Rating System ♟️
summary: Haha you stink!
weight: 6100

# Page metadata.
date: "2025-10-26T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: book  # Do not modify.

---

__William Joe McCann__

## Just how much better am I than you? 🤔

The Elo rating system was developed in the mid-1900s by Arpad Elo[^1] for the purpose of providing a scientific approach to ranking chess players and determining the an appropriate skill vector $\vec{\rho}$ for players. The thought process is straightforward:

1. Take an initial guess at the skill level of a player
2. As new information is gained through matches, update the assumed skill level of the player to reflect this new info

How should we perform the update though? In [this](https://www.youtube.com/watch?v=BT1mmikRils)[^2] Youtube video, creator *dreamingsuntide* walks through an initial model in which you gain $+K$ score every time you win, and $-K$ score every time you lose, with $K$ being some constant. This is simple, but we would want a more robust model as a player could gain skill by beating up on bad players, despite only being mediocre themselves.

As such, let us weigh in the idea of the *gap* between two players' skill levels in order to see how "impressive" it is that one player won over some other player. Of course, we don't know the "true" skill levels of each player (in that case we wouldn't need Elo), so we will be working with the following

> **Definition**: Let $\vec{X_t}$ be our **Elo vector** that represents the our predicted skill values for our set of players $A$ at timestep $t$. Let $\vec{X_\infty}$ represent the predicted skill levels after a sufficiently long time[^3].

What we want is some function that given two skill scores $r_i, r_j\in \vec{X_t}$, will be able to tell us the probability that $i$ wins or that $j$ wins given the gap in their skill. Specifically we want
$$
\begin{align}
\text{Pr}(i\text{ wins}) &= \sigma(r_i-r_j) \\\\
\text{Pr}(j\text{ wins}) &= \sigma(r_j-r_i).
\end{align}
$$

Clearly we have some properties that we want satisfied. If $r_{ij}=r_i-r_j$ then 

1. If $r_i=r_j$, there should be an equal chance of victory. Therefore $\sigma(0)=\frac{1}{2}$
2. $\lim_{r_{ij}\rightarrow\infty} \sigma(r_{ij}) = 1$ 
3. $\lim_{r_{ij}\rightarrow -\infty} \sigma(r_{ij}) = 0$
4. $\sigma(r_{ij})+\sigma(r_{ji})=1\implies \sigma(r_{ij})=1-\sigma(-r_{ij})$ 

Rules (2) and (3) are for ensuring that significantly stronger players have a better chance given of victory, and (4) ensures that order of player in the formula does not matter. There are also alternative formulations that use $-1$ instead of $0$ to represent a loss[^4] but we will stick to $[0,1]$ for now unless specified otherwise.

The standard Elo function, which we will notate as $\sigma_E(r_i-r_j)$ is
$$
\sigma_E(r_i-r_j) = \frac{1}{1+10^{-\frac{r_i-r_j}{s}}}
$$
where $s=400$, which was chosen due to a desired scaling for chess. The update rules for the Elo scores $r_i,r_j$ at time $t$ are then given by
$$
\begin{align}
r_{i,t+1} &= r_{i,t} + K\left(\mathscr{M}(m_t)_i-b\left(r_{i,t}-r_{j,t}\right)\right) \\\\
r_{j,t+1} &= r_{j,t} + K\left(\mathscr{M}(m_t)_j-b\left(r_{j,t}-r_{i,t}\right)\right)
\end{align}
$$
where $\mathscr{M}(m_t)_i$[^5] represents the outcome of the game at timestep $t$ for player $i$, with a $0$ representing a loss, and a $1$ representing a win. $K$ is an arbitrary scaling factor.

## Framing Elo as Stochastic Gradient Descent

An observation that the Elo update rule is just stochastic gradient descent (SGD) is not something new[^6], however my goal here will be to show how all of the constants in the standard Elo function $\sigma_E(r)$ can be mapped to the constants we will be using later, so that practical implementations using $\sigma_E(r)$ have an easy way to convert their values for $K$ and $s$. For some sequence of matches $\mathbb{M}=(m_t)_{t\in\mathbb{N}}$

where $m_t=\left(a_i,a_j\right)\in A\times A$ with the the outcome being shown by function $\mathscr{M}(m_t)$, we want to minimize the log-odds loss function shown here
$$
L(\mathbb{M}) = \sum_{k=1}^|\mathbb{M}| -\log\left(\begin{cases}
\sigma_E(a_i-a_j) & \text{ if i defeats j in m_k} \\\\
\sigma_E(a_j-a_i) & \text{ if j defeats i in m_k}
\end{cases}\right).
$$
What makes this interesting compared to standard SGD is that we will not be randomly selecting datapoints to optimize our function, rather we will be moving sequentially through our list of match outcomes. Using standard SGD formulation we say that in iteration step $t$ then
$$
\vec{X_{t+1}}=\vec{X_{t}}-K\cdot\nabla L(m_t)
$$
where $L(m_t)$ is our loss function only at match $m_t$. Since at any point in time only two objects are participating in a match, all other derivatives will be $0$, and thus we can focus first on $r_{i,t}$
$$
\frac{\partial}{\partial r_i}\left(-\log\left(\frac{1}{1+10^{-\frac{r_i-r_j}{s}}}\right)\right) = -\frac{\log(10)}{s}\frac{1}{1+10^{-\frac{r_j-r_i}{s}}}.
$$
If we simplify our notation, we will see that our update rules become
$$
\begin{cases}
r_{i,t+1} &= r_{i_t}+K\cdot \frac{\log(10)}{s}\sigma_E(r_{j,t}-r_{i,t}) \text{ if i wins} \\\\
r_{i,t+1} &= r_{i_t}-K\cdot \frac{\log(10)}{s}\sigma_E(r_{i,t}-r_{j,t}) \text{ if j wins}
\end{cases}
$$
which is then combined into the version including $\mathscr{M}(m_t)_i$ as shown above.

### Scaling of Variables and Constants

In order to perform the mathematics in a way that is convenient and standard, we will have our update rules as
$$
\vec{X_{t+1}} = \vec{X_t} - \eta\nabla\left(-\log\left((r_{i,t}-r_{j,t})\right)\right)
$$
in which our function $\sigma(r_i-r_j)$ will be
$$
\sigma(r_i-r_j)=\frac{1}{1+e^{-(r_i-r_j)}}
$$
where $X$ will have mean of $0$. Imagine that some person is using the standard Elo algorithm and they have ratings $\overline{r_i},\overline{r_j}$ that has mean of $\mu$, scaled with $s$ factor (again, usually $400$), exponentiation constant of $10$, and some $K$ factor. In order to convert our results to fit their needs, it would be that
$$
K=\frac{s\eta}{\log(10)}
$$
and that
$$
\overliner{r_i} = \frac{s}{log(10)}r_i + \mu.
$$
Interestingly, observing commonly used factors of $K$ as shown on the internet[^7] we can see that selected values of $\eta$ range from around $.09$ to $.25$.


## A Table of Notation

Here is a table of notation for the notation used specifically for this section on Elo. This is an extension of the table of general notation.

1. $\vec{X_t}$ is the vector containing the Elo skill levels for each of our items at timestamp $t$. $\vec{X_\infty}$ represents our Elo vector after a very long time: this may be convergent or sampled from a stationary distribution. 
2. $r_{i,t}\in \vec{X_t}$ is $i$th element of our Elo vector, and the skill level at time $t$ of player $i$. If it is obvious or not necessary to include the timestamp, this will be simplified to $r_i$
3. $r_{ij}=r_i-r_j$ is the gap in skill levels between two players $i$ and $j$. If it is apparent what values of $i,j$ we are using, then this will be denoted simply as $r$. 
4. $\sigma_E$ is the traditional Elo sigmoid function that has scaling parameters $s$ and $K$.
5. $\mu$ is the average Elo score of $X_t$, which is constant across all values of $t$ for a fixed set of players.
6. $\eta$ is the step size parameter of stochastic gradient descent

[^1]: https://uscf1-nyc1.aodhosting.com/CL-AND-CR-ALL/CL-ALL/1967/1967_08.pdf#page=26
[^2]: https://www.youtube.com/watch?v=BT1mmikRils
[^3]: The overhead vector arrow is excluded to avoid me wanting to die while having to continually type $\vec{X}$
[^4]: This allows for $0$ to represent a draw
[^5]: Normally this is represented as the function $S$ which says whether or not the game was a win or loss, however we already use $S$ and will use the piecewise formulation anyway moving forward.
[^6]: https://arxiv.org/pdf/2406.05869
[^7]: https://en.wikipedia.org/wiki/Elo_rating_system#Most_accurate_K-factor