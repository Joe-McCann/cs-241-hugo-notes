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

How should we perform the update though? In [this](https://www.youtube.com/watch?v=BT1mmikRils)[^2] Youtube video, creator *dreamingsuntide* walks through an initial model in which you gain $+k$ score every time you win, and $-k$ score every time you lose, with $k$ being some constant. This is simple, but we would want a more robust model as a player could gain skill by beating up on bad players, despite only being mediocre themselves.

As such, let us weigh in the idea of the *gap* between two players' skill levels in order to see how "impressive" it is that one player won over some other player. Of course, we don't know the "true" skill levels of each player (in that case we wouldn't need Elo), so we will be working with the following

> **Definition**: Let $X_t$ be our **Elo vector** that represents the our predicted skill values for our set of players $T$ at timestep $t$. Let $X$ represent the predicted skill levels after a sufficiently long time[^3].

What we want is some function that given two skill scores $i, j\in X_t$, will be able to tell us the probability that $i$ wins or that $j$ wins given the gap in their skill. Specifically we want
$$
\begin{align}
\text{Pr}(i\text{ wins}) &= b(i-j) \\\\
\text{Pr}(j\text{ wins}) &= b(j-i).
\end{align}
$$

Clearly we have some properties that we want satisfied

1. If $i=j$, there should be an equal chance of victory. Therefore $b(0)=\frac{1}{2}$
2. $\lim_{x\rightarrow\infty} b(x) = 1$ 
3. $\lim_{x\rightarrow -\infty} b(x) = 0$
4. $b(x)=b(-x)$ 

Rules (2) and (3) are for ensuring that significantly stronger players have a better chance given of victory, and (4) ensures that order of player in the formula does not matter. There are also alternative formulations that use $-1$ instead of $0$ to represent a loss[^4] but we will stick to $[0,1]$ for now unless specified otherwise.

The standard Elo function, which we will notate as $b_E(i-j)$ is
$$
b_E(i-j) = \frac{1}{1+10^{-\frac{i-j}{s}}}
$$
where $s=400$, which was chosen due to a desired scaling for chess. The update rules for the Elo scores $i,j$ at time $t$ are then given by
$$
\begin{align}
i_{t+1} &= i_t + K\left(S_{i,t}-b\left(i_t-j_t\right)\right) \\\\
j_{t+1} &= j_t + K\left(S_{j,t}-b\left(j_t-i_t\right)\right)
\end{align}
$$
where $S_{i,t}$ represents the outcome of the game at timestep $t$ for player $i$, with a $0$ representing a loss, and a $1$ representing a win. $K$ is an arbitrary scaling factor.

## Framing Elo as Stochastic Gradient Descent

An observation that the Elo update rule is just stochastic gradient descent (SGD) is not something new[^5], however my goal here will be to show how all of the constants in the standard Elo function $b_E(x)$ can be mapped to the constants we will be using later, so that practical implementations using $b_E(x)$ have an easy way to convert their values for $K$ and $s$. For some sequence of matches 
$$
m_1,m_2,m_3\ldots=\left\[m_k\right]
$$
where $m_k=\left(t_i,t_j\right)\in T\times T$ with the first entry being the winner, and the second entry being the loser, we want to minimize the log-odds loss function shown here
$$
L([m_k]) = \sum_{k=1} -\log\left(\begin{cases}
b_E(X_i-X_j) & \text{ if i defeats j in k} \\\\
b_E(X_j-X_i) & \text{ if j defeats i in k}
\end{cases}\right).
$$
What makes this interesting compared to standard SGD is that we will not be randomly selecting datapoints to optimize our function, rather we will be moving sequentially through our list of match outcomes. Using standard SGD formulation we say that in iteration step $t$ then
$$
X_{t+1}=X_{t}-K\cdot\nabla L_k
$$
where $L_k$ is our loss function only at match $m_k$. In normal Elo we have that $k=t$ but we keep it separate in case future questions call for that. Since at any point in time only two objects are participating in a match, all other derivatives will be $0$, and thus we can focus first on $X_{i,t}$
$$
\frac{\partial}{\partial X_i}\left(-\log\left(\frac{1}{1+10^{-\frac{X_i-X_j}{s}}}\right)\right) = -\frac{\log(10)}{s}\frac{1}{1+10^{-\frac{X_j-X_i}{s}}}.
$$
If we simplify our notation, we will see that our update rules become
$$
\begin{cases}
X_{i,t+1} &= X_{i_t}+K\cdot \frac{\log(10)}{s}b_E(X_{j,t}-X_{i,t}) \text{ if i wins} \\\\
X_{i,t+1} = X_{i_t}-K\cdot \frac{\log(10)}{s}b_E(X_{i,t}-X_{j,t}) \text{ if j wins}
\end{cases}
$$
which is then combined into the version including $S_{i,t}$ as shown above.

### Scaling of Variables and Constants

In order to perform the mathematics in a way that is convenient and standard, we will have our update rules as
$$
X_{t+1} = X_t - \eta\nabla\left(-\log\left(b(X_{i,t}-X_{j,t})\right)\right)
$$
in which our function $b(X_i-X_j)$ will be
$$
b(X_i-X_j)=\frac{1}{1+e^{-(X_i-X_j)}}
$$
where $X$ will have mean of $0$. Imagine that some person is using the standard Elo algorithm and they have ratings $\overline{X_i},\overline{X_j}$ that has mean of $\mu$, scaled with $s$ factor (again, usually $400$), exponentiation constant of $10$, and some $K$ factor. In order to convert our results to fit their needs, it would be that
$$
K=\frac{s\eta}{\log(10)}
$$
and that
$$
\overline{X_i} = \frac{s}{log(10)}X_i + \mu.
$$
Interestingly, observing commonly used factors of $K$ as shown on the internet[^6] we can see that selected values of $\eta$ range from around $.09$ to $.25$.

[^1]: https://uscf1-nyc1.aodhosting.com/CL-AND-CR-ALL/CL-ALL/1967/1967_08.pdf#page=26
[^2]: https://www.youtube.com/watch?v=BT1mmikRils
[^3]: The overhead vector arrow is excluded to avoid me wanting to die while having to continually type $\vec{X}$
[^4]: This allows for $0$ to represent a draw
[^5]: https://arxiv.org/pdf/2406.05869
[^6]: https://en.wikipedia.org/wiki/Elo_rating_system#Most_accurate_K-factor