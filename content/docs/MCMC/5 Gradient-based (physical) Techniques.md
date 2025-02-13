\## Metropolis adjusted Langevin algorithm (MALA).

 

The original **Langevin formula** describes Brownian motion. Due to the collision of fluid molecules, particles move irregularly in the fluid

{{< katex display=true >}}

m\frac{d^2x}{dt^2}=-\lambda\frac{dx}{dt}+\eta(t)

{{< /katex >}}

where {{< katex >}}-\lambda\frac{dx}{dt}{{< /katex >}} represents the fluid viscosity, {{< katex >}}\eta(t){{< /katex >}} represents the collision force caused by molecular thermal motion, also known as thermal fluctuation.

 

Obtain the solution

{{< katex display=true >}}

\bar{x^2}=2Dt=2\frac{k_BT}{\lambda}t

{{< /katex >}}

 

here, {{< katex >}}D=\frac{k_BT}{\lambda}{{< /katex >}}, which means the square of the distance between the average motion position of Brownian motion particles and the origin (initial point) is proportional to time.

 

NOTE：式子的解表示粒子能跑到的范围

 

**Simulated annealing (SA)** is a probabilistic technique for approximating the global optimum of a given function. Specifically, it is a metaheuristic to approximate global optimization in a large search space for an optimization problem. 

 

The state of some physical systems, and the function {{< katex >}}E(s) {{< /katex >}} to be minimized, is analogous to the internal energy of the system in that state. The goal is to bring the system, from an arbitrary initial state, to a state with the minimum possible energy.

 

**Algorithm Metropolis adjusted Langevin algorithm (MALA).**

 

$(1)$ Choose an initial state {{< katex >}}\theta^{(0)} {{< /katex >}}, the discretization step $\Delta\tau$, the total number of iterations {{< katex >}} (T) {{< /katex >}}, and the burn-in period {{< katex >}} (T_b) {{< /katex >}}.

 

$(2)$ FOR {{< katex >}}t=1,\cdots,T{{< /katex >}}:\\

(a) Draw $z\sim N(0,I), u\sim U([0,1))$ and simulate a new sample from the Langevin diffusion:

{{< katex display=true >}}

\theta'=\theta^{(t-1)}+\frac{\Delta\tau}{2}\nabla log\pi(\theta^{(t-1)})+\sqrt{\Delta\tau}z

{{< /katex >}}

 

(b) Compute the acceptance probability ($\alpha_t$):

{{< katex display=true >}}

\alpha(\theta',\theta^{(t-1)})=min[1,\frac{\pi(\theta')N(\theta^{(t-1)}|\theta'+\frac{\Delta\tau}{2}\nabla log\pi(\theta')),\Delta\tau I} {\pi(\theta^{(t-1)})N(\theta'|\theta^{(t-1)}+\frac{\Delta\tau}{2}\nabla log\pi(\theta^{(t-1)})),\Delta\tau I}]

{{< /katex >}}

 

(c)If {{< katex >}}u\leq \alpha_t{{< /katex >}}, accept $\theta'$ and set {{< katex >}}\theta^{(t)}=\theta'{{< /katex >}}. Otherwise (i.e., if {{< katex >}}u>\alpha_t{{< /katex >}}), reject {{< katex >}}\theta'{{< /katex >}} and set {{< katex >}}\theta^{(t)}=\theta^{(t-1)}{{< /katex >}}.

 

$(3)$  Approximate the integral

{{< katex display=true >}}

\hat{I}_{(T-T_b)}=\frac{1}{T-T_b}\sum_{t=T_b+1}^{T}g(\theta^{(t)})

{{< /katex >}}

NOTE：原始的退火方程为

{{< katex display=true >}}

d\theta(\tau)=\frac{1-2d}{2}\pi_u^{1-2d}(\theta(\tau))\nabla log\pi_u \theta(\tau)+\pi_u^{-d}(\theta(\tau))db(\tau)

{{< /katex >}}

其中{{< katex >}}d{{< /katex >}}的取值范围是{{< katex >}}[0,\frac{1}{2}]{{< /katex >}}，表示温度的高低，在最终稳态时温度取最低也就是{{< katex >}}d=0{{< /katex >}}；{{< katex >}}\pi_u{{< /katex >}}表示扩散方程，因此算法可简化为如上形式。

 

 

\## Algorithm Hamiltonian Monte Carlo Algorithm (HMC).

 

Hamiltonian

{{< katex display=true >}}

H(\theta,\rho)=-log\pi(\theta)+\frac{1}{2}\rho^T\rho

{{< /katex >}}

 

{{< katex display=true >}}

p(\theta,\rho)=\frac{1}{Z}exp(-H(\theta,\rho))=\bar{\pi}(\theta)N(\rho|0,I)

{{< /katex >}}

 

The Hamiltonian equations for the dynamics of the particles in fictitious time $\tau$ are now given by

{{< katex display=true >}}

\frac{d\theta}{d\tau}=\nabla_\rho H=\rho

{{< /katex >}}

 

{{< katex display=true >}}

\frac{d\rho}{d\tau}=-\nabla_\theta H=\nabla log\pi(\theta)

{{< /katex >}}

 

The HMC algorithm constructs the proposal distribution by simulating trajectories from the Hamiltonian equations.

 

NOTE：采用辛积分器计算结果，例如蛙跳法

 

**Algorithm Hamiltonian Monte Carlo (HMC) Algorithm.**

 

$(1)$ Choose an initial state $\theta^{(0)}$, the discretization step {{< katex >}}\Delta\tau{{< /katex >}}, the number of integration steps {{< katex >}}L{{< /katex >}}, the total number of iterations {{< katex >}} (T) {{< /katex >}}, and the burn-in period {{< katex >}} (T_b) {{< /katex >}}.

 

$(2)$ FOR {{< katex >}}t=1,\cdots,T{{< /katex >}}:

 

(a) Draw {{< katex >}}u\sim U([0,1)) {{< /katex >}}, numerically solve the Hamiltonian equations, using $L$ steps of a Leapfrog method starting from {{< katex >}}\tilde{\theta}^{(0)}=\theta^{(t-1)} {{< /katex >}} and {{< katex >}}\tilde{\rho}^{(0)}\sim N(0,I) {{< /katex >}}, setting {{< katex >}}\theta'=\tilde{\theta}^{(L\Delta\tau)} {{< /katex >}} and {{< katex >}}\rho'=-\tilde{\rho}^{(L\Delta\tau)} {{< /katex >}}.

 

(b) Compute the acceptance probability:

{{< katex display=true >}}

\begin{align*}

   \alpha_t&=\alpha(\theta',\rho';\theta^{(t-1)},\rho_{t-1}) \\

​            &=min[1,exp(-H(\theta',\rho')+H(\theta^{(t-1)},\rho_{t-1})))]

 \end{align*}

{{< /katex >}}

(c) If $u\leq \alpha_t$, accept $\theta'$ and set $\theta^{(t)}=\theta'$. Otherwise, reject $\theta'$ and set $\theta^{(t)}=\theta^{(t-1)}$

 

$(3)$ Approximate the integral using

{{< katex display=true >}}

\hat{I}_{(T-T_b)}=\frac{1}{T-T_b}\sum_{t=T_b+1}^{T}g(\theta^{(t)})

{{< /katex >}}

 

![HMC1](E:\学习\研二\Pres\Summer work\7.30\MCMC3\Figure\HMC1.png)

 

NOTE：可将分布翻转，分布的峰看做碗底，HMC就是在一个无摩擦的碗里拨动一个小球，小球的每次停止位置即为一个采样，[可看直观图像](http://elevanth.org/blog/2017/11/28/build-a-better-markov-chain/)

 

 

\## Riemann manifold MALA and HMC

 

The idea of the **Riemann manifold Langevian and Hamiltonian Monte Carlo** methods is to perform the Langevin or Hamiltonian simulations in a suitable Riemann manifold instead of the Euclidean space.

 

The squared distance between two locations $\theta$ and $\theta+d\theta$ in Euclidean space is given by

{{< katex display=true >}}

d^2=d\theta^Td\theta

{{< /katex >}}

In the Riemann manifold, the distance is generalized to be

{{< katex display=true >}}

d^2=d\theta^TG^{-1}(\theta)d\theta

{{< /katex >}}

where {{< katex >}}G(\theta) {{< /katex >}} is the metric tensor, which is a positive definite matrix for any given {{< katex >}}\theta{{< /katex >}}.

 

\## MMALA

 

We can now modify the MALA method such that the SDE evolves along the Riemann manifold instead of the Euclidean space as follows:

{{< katex display=true >}}

d\theta(\tau)=\tilde{f}(\theta(t))d\tau+d\tilde{b}(\tau)

{{< /katex >}}

where

{{< katex display=true >}}

\tilde{f}(\theta)=\frac{1}{2}G^{-1}(\theta)\nabla log\pi(\theta)

{{< /katex >}}

 

{{< katex display=true >}}

d\tilde{b}_i=|G(\theta)|^{-1/2}\sum_{j}^{}\frac{\partial}{\partial\theta_j}[G^{-1}(\theta)_{ij}]|G(\theta)|^{-1/2}dt+[G^{-1/2}(\theta)db]_i

{{< /katex >}}

 

The MMALA algorithm can now be constructed by replacing the SDE in the basic MALA with the SDE defined above.

 

\## RMHMC

 

In the Riemann manifold Hamiltonian Monte Carlo (RMHMC) we construct the particle system dynamics in the Riemann manifold. This results in the following Hamiltonian:

{{< katex display=true >}}

H(\theta,\rho)=-log\pi(\theta)+\frac{1}{2}log[2\pi G(\theta)]+\frac{1}{2}\rho^TG^{-1}(\theta)\rho

{{< /katex >}}

and the Hamiltonian equations are now given as

{{< katex display=true >}}

\frac{d\theta}{d\tau}=\nabla_\rho H=G^{-1}\theta\rho

{{< /katex >}}

 

{{< katex display=true >}}

\frac{d\rho}{d\tau}=-\nabla_\theta H=\nabla log\pi(\theta)+h(\theta)

{{< /katex >}}

 

where the additional term is given by

{{< katex display=true >}}

h_i(\theta)=-\frac{1}{2}tr\left\{G^{-1}(\theta)\frac{\partial G(\theta)}{\partial\theta_i}\right\}+\frac{1}{2}\rho^T G^{-1}\theta\frac{\partial G(\theta)}{\partial\theta_i}G^{-1}(\theta)\rho

{{< /katex >}}

 

 

\## "no-U-turn sampler" (NUTS).

 

In practice, it is important to note that discretizing the Hamiltonian dynamics introduces two free parameters, the step size {{< katex >}}\epsilon{{< /katex >}} and the trajectory length $T\epsilon$, both to be calibrated. As an empirically successful and popular variant of HMC, the "no-U-turn sampler" (NUTS) of Hoffman and Gelman (2014) adapts the value of {{< katex >}}\epsilon{{< /katex >}} based on primal-dual averaging.[^1]

 

It also eliminates the need to choose the trajectory length {{< katex >}}T{{< /katex >}} via a recursive algorithm that builds a set of candidate proposals for a number of forward and backward leapfrog steps and stops automatically when the simulated path steps back.

 

![NUTS](https://github.com/MokouTIde/MCMC-accelerating.github.io/blob/master/Figure/NUTS1.png)

 

Notice how the path grows in both directions. This is the algorithm figuring out when the path turns around. When the path starts to turn around, NUTS stops the simulation and takes a sample. Then it flicks the particle in another random direction and starts another simulation.

 

[^1] [Hoffman, Matthew D., and Andrew Gelman. "The No-U-Turn sampler: adaptively setting path lengths in Hamiltonian Monte Carlo." J. Mach. Learn. Res. 15.1 (2014): 1593-1623.](https://www.jmlr.org/papers/volume15/hoffman14a/hoffman14a.pdf)