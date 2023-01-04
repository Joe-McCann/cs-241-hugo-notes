---
title: "Answers to Followup Questions: Modeling Epidemics with SIR Models"
linktitle: " 🥵🥶🤢 Epidemics Modeling Followup Answers"
toc: true
type: book
draft: true
weight: 308
---

## Answers to followup questions

#### Question 1

Show that $\frac{\mathrm{d}}{\mathrm{d}t} (S+I+R)=0$, so that if $S(0)+I(0)+R(0)=N$ then $S(t)+I(t)+R(t)=N$ for all $t$.

##### Answer

$$
\begin{aligned}
\frac{\mathrm{d}}{\mathrm{d}t} (S+I+R)&= 
\frac{\mathrm{d}S}{\mathrm{d}t}+ \frac{\mathrm{d}I}{\mathrm{d}t}+\frac{\mathrm{d}R}{\mathrm{d}t}\\
&= -\beta S I + (\beta S I - \gamma I) + \gamma I\\
&= 0.
\end{aligned}
$$



#### Question 2

* Where would the imposition or a stay-at-home order effect this model?

  _As people stay at home, they have less contact with others, which decreases the contact rate $c$ and thus of $\beta$._

#### Question 3

* Where would widespread adoption of mask wearing effect this model?

  _The more people wear masks, the lower the probability that any individual contact leads to transmission. That is, it reduces $p$ and thus reduces $\beta$._ 

#### Question 4

* Come up with a feature that you think is missing from the model but straightforward to add to the model, and propose a modification to the model that you think would incorporate this feature. In particular, identify any new variables or terms needed for you modified model.

  _There are many features we know about the disease that we have simplified out of the model. I'll focus on two:_

    1. An important fact about COVID is that it has what's estimated to be a two-week incubation period before infected individuals show symptoms and that infected people are most likely to pass on the disease for a few days before and after the onset of symptoms. Expressing this relationship precisely using mathematics is somewhat beyond the scope of this class. 

       * One way to do this might be to make the rate at which individuals transition from susceptible to infected proportional to a _delayed_ value $S(t-\tau)I(t-\tau).$ Solving such _delay differential equations_ (DDE) is tough, both theoretically and numerically. 

       * Another way to do this is to introduce a fourth group of individuals, the _exposed_ group, represented by $E(t)$. Individuals now move from state to state like this:

         

         ```mermaid
         graph LR;
           S((S))-- β S I -->E((E));
           E -- c E --> I((I))
           I-- ɣ I -->R((R));
         ```

         Now, of course the number of equations is larger so we can't plot things nicely in a two-dimensional phase plane anymore.

    2. Another assumption that we could remove is the assumption of a well-mixed population. In a gross oversimplification, we can divide the population into two subpopulations. __Subpopulation A__, the "safe" group, is able and willing to follow safety precautions. __Subpopulation B__ the "risky" group, engages in more risky behavior, either because their economic situation forces them to work a riskier job or because they ignore the recommendations of public health experts. The rates at which __A__ people and __B__ people meet each other and the probability that a contact leads to transmission are different. Further, type **B** people receive higher viral loads and, on average, get sicker, so the rate at which they recover is slower.

       There are some places everyone may meet (the grocery store), and there are some places only __A__ type people go and some places only __B__ type people go (they might have to work in a meat-processing plant, or they might be going to underground dance parties, or who knows where). They also have different rates of recovery.
       $$
       \begin{aligned}
       \frac{\mathrm{d}S_A}{\mathrm{d}t}&=-\beta_{AA}S_A I_A - \beta_{BA} S_A I_B, &\qquad
       \frac{\mathrm{d}S_B}{\mathrm{d}t}&=-\beta_{AB}S_B I_A - \beta_{BB} S_B I_B, \\\\
       \frac{\mathrm{d}I_A}{\mathrm{d}t}&=\beta_{AA}S_A I_A + \beta_{BA} S_A I_B -\gamma_A I_A,&\qquad
       \frac{\mathrm{d}I_B}{\mathrm{d}t}&=\beta_{AB}S_B I_A + \beta_{BB} S_B I_B -\gamma_B I_B, \\\\
       \frac{\mathrm{d}R_A}{\mathrm{d}t}&=-\gamma_A I_A, &
       \frac{\mathrm{d}R_B}{\mathrm{d}t}&=-\gamma_B I_B.
       \end{aligned}
       $$

    

    Note that $\beta_{BA}$ is involved in the transmission of the pathogen from the $B$ subpopulation to the  $A$ subpopulation, while $\beta_{AB}$ measures transmission rate from the $A$ subpopulation to the $B$ subpopulation. There are a lot of constants to figure out in this model!

  3. You may have come up with your own idea, and that's great too. 