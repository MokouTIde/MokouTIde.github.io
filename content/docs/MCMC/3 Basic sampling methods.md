## Metroplis Algorithm

$(1)$ Draw {{< katex >}}\theta{{< /katex >}} from {{< katex >}}q(\theta){{< /katex >}} and denote the draw by {{< katex >}}\theta^*{{< /katex >}}.

$(2)$ Compute the ratio 
{{< katex display=true >}}
\alpha=\frac{p(\theta^*|y)}{Kq(\theta^*)}\quad (1\quad or\quad exp(-\Delta E/kT)).
{{< /katex >}}

$(3)$ With probability {{< katex >}}\alpha{{< /katex >}}, accept the draw {{< katex >}}\theta^*{{< /katex >}} as a draw from the posterior,{{< katex >}}p(\theta^*|y){{< /katex >}}. If {{< katex >}}\theta^*{{< /katex >}} is rejected, go back to step {{< katex >}}(1){{< /katex >}}. 

To decide whether to accept or not, draw one observation, {{< katex >}}u{{< /katex >}}, from the uniform distribution on {{< katex >}}U(0,1){{< /katex >}}. If {{< katex >}}u\leqslant \alpha{{< /katex >}}, accept {{< katex >}}\theta^*{{< /katex >}}; If {{< katex >}}u\geqslant a{{< /katex >}}, reject {{< katex >}}\theta^*`{{< /katex >}}.

NOTE：Metropolis算法最早用于求粒子系统的内能，因此{{< katex >}}(2){{< /katex >}}中出现了{{< katex >}}exp(-\Delta E/kT){{< /katex >}}项


## Metroplis-Hasitings Algorithm

$(1)$ Initiate the algorithm with a value {{< katex >}}\theta^{(0)}{{< /katex >}} from the parameter space of {{< katex >}}\theta{{< /katex >}}.

$(2)$ At iteration {{< katex >}}t{{< /katex >}}, draw a (multivariate) realization, {{< katex >}}\theta^*{{< /katex >}}, from the proposal density, {{< katex >}}q(\theta|\theta^{(t-1)}){{< /katex >}}, where {{< katex >}}\theta^{(t-1)}{{< /katex >}} is the parameter value at the previous step.

$(3)$ Compute the acceptance probability, given by
{{< katex display=true >}}
\alpha(\theta^*,\theta^{(t-1)})=min\left\{1,\frac{p(\theta^*)/q(\theta^*|\theta^{(t-1)})}{p(\theta^{(t-1)})/q(\theta^{(t-1)}|\theta^*)}\right\}
{{< /katex >}}
where we suppress notationally the dependence on the data, {{< katex >}}y{{< /katex >}}, for simplicity.

$(4)$ Draw {{< katex >}}u{{< /katex >}} from the uniform distribution on {{< katex >}}U(0, 1){{< /katex >}}. Then,

If {{< katex >}}u\leqslant \alpha(\theta^*,\theta^{(t-1)}){{< /katex >}}, set {{< katex >}}\theta^{(t)}=\theta^*{{< /katex >}}.

Otherwise, set {{< katex >}}\theta^{(t)}=\theta^{(t-1)}{{< /katex >}}.

$(5)$ Go back to step {{< katex >}}(2){{< .katex >}}.

## Gibbs sampler

$(1)$ Initialize the chain by selecting the starting values for all components, {{< katex >}}\theta_i^{(0)},i=1,2,\cdots,q{{< /katex >}}.

$(2)$ At iteration {{< katex >}}t{{< /katex >}}, obtain the draw of {{< katex >}}\theta=(\theta_1,\theta_2,\cdots,\theta_q){{< /katex >}} by drawing and updating successively its components, as follows:

Draw an observation, {{< katex >}}\theta_1^{(t)}{{< /katex >}} from {{< katex >}}p(\theta_1|\theta_2^{(t-1)},\theta_3^{(t-1)},\cdots,\theta_q^{(t-1)},y){{< /katex >}},

Draw an observation, {{< katex >}}\theta_2^{(t)}{{< /katex >}} from {{< katex >}}p(\theta_2|\theta_1^{(t)},\theta_3^{(t-1)},\cdots,\theta_q^{(t-1)},y){{< /katex >}}

Cycle through the rest of the components, {{< katex >}}\theta_3,\cdots,\theta_q{{< /katex >}}, in a similar way.

$(3)$ Repeat step {{< katex >}}(2){{< /katex >}} until convergence is achieved.

NOTE：Gibbs采样中，可分为三类，分别是一般Gibbs采样（Generic Gibbs sampler），系统扫描Gibbs采样（Systematic scan Gibbs sampler），对称扫描Gibbs采样（Symmetric scan Gibbs sampler），精度有提升的同时计算量也增加，因此整体效率提升不大。
