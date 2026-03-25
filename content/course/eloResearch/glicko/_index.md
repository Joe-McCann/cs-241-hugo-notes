---
# Course title, summary, and position.
title: The Glicko Rating System 🔔
linktitle: Glicko Rating 🔔
summary: Its Bayes Time Baby!
weight: 6200

# Page metadata.
date: "2025-10-26T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: false  # Show table of contents? true/false
type: book  # Do not modify.

---

__William Joe McCann__

## And I Can Say it's What You Know 🎵

In the early $90$s, the method of Elo for ranking was being critically evaluated due to a seemingly large gap relating to the scaling $K$ factor. When new players join in to the league, we have no idea how good they are and thus want a larger $K$ factor to represent us moving to their "true" skill quicker. As we gain more information, we want to shrink our $K$ factor to represent smaller shifts in Elo score, assuming we are close to their "true" skill at this point. Some implementations of Elo do this by having different $K$ factors depending on the number of games played.

However, say a player $a_i$ achieves some Elo score of $r_i$ and then drops off the planet for a whole year. When they come back, should we treat them as a new player, or someone who already has a lot of experience? We have no idea if they spent that time training, decaying, or are about the same.

The **Glicko**[^1] system was designed with this in mind, taking into account player skill variance as a parameter that affects how we determine skill. Allegedly, this is the first Bayesian based ranking system.

Unlike Elo which is a single-step stochastic process in which each match has immediate impact, Glicko works with what are called "rating periods", in which all the matches during $T$ units of time are compiled and then used to adjust the ratings of players at the end of the period. If we were to observe another match from this player $t'$ units of time after the most recent rating we have for them at time $t$, we would assume their skill to be a random variable normally distributed around their most recently observed rating. Specifically 
$$
R_{i,t+t'} | R_{i,t},\nu^2,t \sim \mathcal{N}(R_{i,t},\nu^2t').
$$
Observe the parameter $\nu^2$ that snuck its way inside there for our variance. We represent $\nu^2$ to be the amount of variance added per unit time that we do not see the player competing. We use this to put a numerical quantity around how much uncertainty we gain when we don't see a player compete for a long while. This is a model parameter and is pre-selected to be fixed.

Its also important to note that while $R_{i,t}$ is used as the mean, however 

When some player $a_i$ plays against player $a_j$, we assume that the strengths of both players are normally distributed around some mean with some variance that is provided from the start of the rating period at time $t$

$$
\begin{align}
R_{i,t} &| \mu_{i,t},\sigma^2_{i,t} \sim \mathcal{N}(\mu_{i,t},\sigma^2_{i,t}) \\\\
R_{j,t} &| \mu_{j,t},\sigma^2_{j,t} \sim \mathcal{N}(\mu_{j,t},\sigma^2_{j,t}).
\end{align}
$$

We now need to understand what the probability of a specific outcome occurring given the ratings of players $r_i, r_j$. The model in general does not require the game to be specifically a win-loss game, and allows for ties. Using our notation from the previous page, if match $m=(r_i, r_j)$ then for the Glicko algorithm $\mathscr{M}(m)_i\in [0,1]$. This means that the outcome of our game for player $i$ *in general* does not need to be $0,1$, for example they represent a tie as $.5$[^2]. As such their generalized sigmoid *likelihood* function is

$$
\sigma_G(r_i, r_j, m) = \frac{\left(10^{\frac{r_i-r_j}{400}}\right)^{\mathscr{M}(m)_i}}{1+10^{\frac{r_i-r_j}{400}}}.
$$

In the event that the game is a win-loss game with $0$ being a loss and $1$ being a win, we get the sigmoid functions and dynamics from the Elo system. Written out in the same piecewise version as before

$$
\sigma_G(r_i, r_j, m) = \begin{cases}
\sigma_E(r_i-r_j) & \text{ if i defeats j in m} \\\\
\sigma_E(r_j-r_i) & \text{ if j defeats i in m}.
\end{cases}
$$

## The Glicko Formula



## Table of Notation

1. $\sigma_G(r_1-r_2)$ is the sigmoid function for the Glicko algorithm
2. $r_{i,t}$: The estimated rating of player $i$ at timestamp $t$, note that this is the same notation as before, however in the Glicko paper the notation $\theta^{t}_i$ is used instead. We use capital $R$ to denote the random variable. 
3. $T$ is the length of $1$ rating period in units of time 
4. $\nu^2$ is the variance per unit time added to a players skill distribution to represent uncertainty
5. $\mathcal{N}(\mu, \sigma^2)$ represents a normal distribution centered around $\mu$ with variance $\sigma^2$. The function $N(x|\mu, \sigma^2)$ is the pdf of a normal distribution given those mean and variance values.

[^1]: https://www.glicko.net/research/glicko.pdf
[^2]: We will not consider the case of ties for the sake of research for now.