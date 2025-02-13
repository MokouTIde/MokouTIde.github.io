## Metroplis Algorithm

$(1)$ Draw $\theta$ from $q(\theta)$ and denote the draw by $\theta^*$.

$(2)$ Compute the ratio 
{{< katex display=true >}}
\alpha=\frac{p(\theta^*|y)}{Kq(\theta^*)}\quad (1\quad or\quad exp(-\Delta E/kT)).
{{< /katex >}}

$(3)$ With probability $\alpha$, accept the draw {{< katex >}}\theta^*{{< /katex >}} as a draw from the posterior,{{< katex >}}p(\theta^*|y)$. If $\theta^*{{< /katex >}} is rejected, go back to step $(1)$. 

To decide whether to accept or not, draw one observation, $u$, from the uniform distribution on $U(0,1)$. If {{< katex >}}u\leqslant \alpha{{< /katex >}}, accept {{< katex >}}\theta^*{{< /katex >}}; If {{< katex >}}u\geqslant a{{< /katex >}}, reject {{< katex >}}\theta^*`{{< /katex >}}.

NOTE：Metropolis算法最早用于求粒子系统的内能，因此{{< katex >}}(2){{< /katex >}}中出现了{{< katex >}}exp(-\Delta E/kT){{< /katex >}}项


## Metroplis-Hasitings Algorithm

$(1)$ Initiate the algorithm with a value $\theta^{(0)}$ from the parameter space of $\theta$.

$(2)$ At iteration $t$, draw a (multivariate) realization, $\theta^*$, from the proposal density, $q(\theta|\theta^{(t-1)})$, where $\theta^{(t-1)}$ is the parameter value at the previous step.

$(3)$ Compute the acceptance probability, given by
{{< katex display=true >}}
\alpha(\theta^*,\theta^{(t-1)})=min\left\{1,\frac{p(\theta^*)/q(\theta^*|\theta^{(t-1)})}{p(\theta^{(t-1)})/q(\theta^{(t-1)}|\theta^*)}\right\}
{{< /katex >}}
where we suppress notationally the dependence on the data, $y$, for simplicity.

$(4)$ Draw $u$ from the uniform distribution on $U(0, 1)$. Then,

If {{< katex >}}u\leqslant \alpha(\theta^*,\theta^{(t-1)}){{< /katex >}}, set $\theta^{(t)}=\theta^*$.

Otherwise, set {{< katex >}}\theta^{(t)}=\theta^{(t-1)}{{< /katex >}}.

$(5)$ Go back to step $(2)$.

## Gibbs sampler

$(1)$ Initialize the chain by selecting the starting values for all components, $\theta_i^{(0)},i=1,2,\cdots,q$.

$(2)$ At iteration $t$, obtain the draw of $\theta=(\theta_1,\theta_2,\cdots,\theta_q)$ by drawing and updating successively its components, as follows:

Draw an observation, $\theta_1^{(t)}$ from $p(\theta_1|\theta_2^{(t-1)},\theta_3^{(t-1)},\cdots,\theta_q^{(t-1)},y)$

Draw an observation, $\theta_2^{(t)}$ from $p(\theta_2|\theta_1^{(t)},\theta_3^{(t-1)},\cdots,\theta_q^{(t-1)},y)$

Cycle through the rest of the components, $\theta_3,\cdots,\theta_q$, in a similar way.

$(3)$ Repeat step $(2)$ until convergence is achieved.

NOTE：Gibbs采样中，可分为三类，分别是一般Gibbs采样（Generic Gibbs sampler），系统扫描Gibbs采样（Systematic scan Gibbs sampler），对称扫描Gibbs采样（Symmetric scan Gibbs sampler），精度有提升的同时计算量也增加，因此整体效率提升不大。
