---
title: Basic MCMC
weight: 1
---


# BasicMCMC


## Metropolis-Hastings Update


Specified distribution has *unnormalized density* $h$,that is, $h$ is a positive constant ties a probability density. The MH update does:

- When current state is $x$, propose a move to $y$, having conditional probability density given $x$ denoted $q(x,\cdot)$.
- Calculate the Hastings ratio
{{< katex display=true >}}
r(x, y)=\frac{h(y) q(y, x)}{h(x) q(x, y)}
{{< /katex >}}
- Accept the proposed move $y$ with probability 
$$
a(x, y)=\min (1, r(x, y))
$$
That is, the state after the update is $y$ with probability $a(x,y)$, and the state after the update is $x$ with probability $1-a(x,y)$.
