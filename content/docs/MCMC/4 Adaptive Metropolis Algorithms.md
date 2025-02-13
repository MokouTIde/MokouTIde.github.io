## AM

**Algorithm General Adaptive Metropolis (AM) Algorithm.**

$(1)$ Initialization: Choose an initial covariance {{< katex >}}\Sigma_0{{< /katex >}}, an initial adaptation parameter {{< katex >}}\lambda_0{{< /katex >}}, a target acceptance rate {{< katex >}}\bar{\alpha}{{< /katex >}} , an initial state {{< katex >}}\theta^{(0)} {{< /katex >}}, the total number of iterations {{< katex >}} (T) {{< /katex >}} and the burn-in period {{< katex >}} (T_b) {{< /katex >}}.

$(2)$ FOR {{< katex >}}t=1,\cdots,T{{< /katex >}}:

(a) Set $C_t=\lambda_{t-1}\Sigma_{t-1}{{< /katex >}}. Draw {{< katex >}}\theta'\sim N(\theta^{(t-1)},C_t) {{< /katex >}} and {{< katex >}}u\sim U([0,1)) {{< /katex >}}.

(b) Compute the acceptance probability:

{{< katex display=true >}}
\alpha_t\equiv \alpha(\theta',\theta^{(t-1)})=min[1,\frac{\pi(\theta')}{\pi(\theta^{(t-1)})}]
{{< /katex >}}

(c) If {{< katex >}}u\leq \alpha_t{{< /katex >}}, accept {{< katex >}}\theta'{{< /katex >}} and set {{< katex >}}\theta^{(t)}=\theta'{{< /katex >}}. Otherwise, reject {{< katex >}}\theta'{{< /katex >}} and set {{< katex >}}\theta^{(t)}=\theta^{(t-1)} {{< /katex >}}

(d) Obtain a new estimate of the target’s covariance {{< katex >}}\Sigma_t{{< /katex >}}, e.g., by using estimator of

{{< katex display=true >}}
\Sigma_t=Cov[\theta^{(0)},\cdots,\theta^{(t-1)},\theta^{(t)}]+\epsilon I.
{{< /katex >}}

(e) Compute a new {{< katex >}}\lambda_t{{< /katex >}}, e.g., by apllying the rule of {{< katex >}}log\lambda_t=log\lambda_{t-1}+\gamma_t(\alpha_t-\bar{\alpha}){{< /katex >}}._

$(3)$ Approximate the integral using

{{< katex display=true >}}
\hat{I}_{(T-T_b)}=\frac{1}{T-T_b}\sum_{t=T_b+1}^{T}g(\theta^{(t)})
{{< /katex >}}






