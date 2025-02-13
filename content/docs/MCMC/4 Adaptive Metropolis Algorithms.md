## Algorithm General Adaptive Metropolis Algorithm (AM).

$(1)$ Initialization: Choose an initial covariance {{< katex >}}\Sigma_0{{< /katex >}}, an initial adaptation parameter {{< katex >}}\lambda_0{{< /katex >}}, a target acceptance rate {{< katex >}}\bar{\alpha}{{< /katex >}} , an initial state {{< katex >}}\theta^{(0)} {{< /katex >}}, the total number of iterations {{< katex >}} (T) {{< /katex >}} and the burn-in period {{< katex >}} (T_b) {{< /katex >}}.

$(2)$ FOR {{< katex >}}t=1,\cdots,T{{< /katex >}}:

(a) Set {{< katex >}}C_t=\lambda_{t-1}\Sigma_{t-1}{{< /katex >}}. Draw {{< katex >}}\theta'\sim N(\theta^{(t-1)},C_t) {{< /katex >}} and {{< katex >}}u\sim U([0,1)) {{< /katex >}}.

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


## ARS

Assume that we want to draw samples from a target {{< katex >}}pdf{{< /katex >}}, with support {{< katex >}}D_{\theta}\subseteq \mathbb{R}{{< /katex >}}, known up a normalization constant. The ARS procedure can be applied when {{< katex >}}\pi(\theta){{< /katex >}} is log-concave, i.e. when
{{< katex display=true >}}
V(\theta)\triangleq log(\pi(\theta))
{{< /katex >}}
is strictly concave {{< katex >}}\forall \theta\in D_{\theta}\subseteq \mathbb{R}{{< /katex >}}. In this case, let
{{< katex display=true >}}
S^{(0)}\triangleq \left\{\theta_1^{(0)},\theta_2^{(0)},\cdots,\theta_{K_0}^{(0)}\right\} \subset D_{\theta}
{{< /katex >}}

be the set of support points at time {{< katex >}}0{{< /katex >}}, sorted in ascending order (i.e. {{< katex >}}\theta_1^{(0)}<\cdots<\theta_{K_0}^{(0)}{{< /katex >}}).
From {{< katex >}}S^{(m)}{{< /katex >}} a piecewise-linear function {{< katex >}}W_m(\theta){{< /katex >}} is constructed such that

{{< katex display=true >}}
W_m(\theta)\geq V(\theta)
{{< /katex >}}

for all {{< katex >}}\theta\in D_{\theta}{{< /katex >}} and {{< katex >}}\forall t\in \mathbb{N}{{< /katex >}}. Different approaches are possible for constructing this function. For instance, if we denote by {{< katex >}}w_k(\theta) {{< /katex >}} the linear function tangent to {{< katex >}}V(\theta){{< /katex >}} at {{< katex >}}\theta\in S{{< /katex >}}, then a suitable piecewise linear function {{< katex >}}W_m(\theta){{< /katex >}} is:

{{< katex display=true >}}
W_m(\theta)\triangleq min\left\{w_1(\theta),\cdots,w_{m_t}(\theta)\right\}\quad for\quad \theta\in D_{\theta}
{{< /katex >}}

Indeed, defining the intervals

{{< katex display=true >}}
\mathcal{I}_0\triangleq (-\infty,\theta_1^{(m)}],\mathcal{I}_1\triangleq (\theta_1^{(m)},\theta_2^{(m)}],\cdots,\mathcal{I}_{K_m-1}\triangleq (\theta_{K_m-1}^{(m)},\theta_{K_m}^{(m)}],\mathcal{I}_{K_m}\triangleq (\theta_{K-m}^{(m)},+\infty)
{{< /katex >}}

and denoting as {{< katex >}}L_{i,i+1}(\theta){{< /katex >}} the straight line passing through the points {{< katex >}}(\theta_i,V(\theta_i)){{< /katex >}} and {{< katex >}}\theta_{i+1},V(\theta_{i+1}){{< /katex >}} for {{< katex >}}1\leq i\leq K_m{{< /katex >}}, a suitable function {{< katex >}}W_m(\theta){{< /katex >}} can be expressed as

{{< katex display=true >}}
   W_m(\theta)\triangleq \left\{
    \begin{array}{ccc}
      L_{1,2}(\theta) &  & \theta\in\mathcal{I}_0^{(m)}=(-\infty,\theta_1^{(m)}]  \\
      L_{2,3}(\theta) &  & \theta\in\mathcal{I}_1^{(m)}=(\theta_1^{(m)},\theta_2^{(m)}] \\
      min\left\{L_{j-1,j}(\theta),L_{j+1,j+2}(\theta)\right\} &  & \theta\in\mathcal{I}_j^{(m)}=(\theta_{j}^{(m)},\theta_{j+1}^{(m)}],2\leq j\leq m_t-2 \\
      L_{m_t-2,m_t-1} &  & \theta\in\mathcal{I}_{K_m-1}^{(m)}=(s_{m_t-1},s_{m_t}] \\
      L_{m_t-1,m_t} &  & \theta\in\mathcal{I}_{K_m}^{(m)}=\theta_{K-m}^{(m)},+\infty)
    \end{array} \right.
{{< /katex >}}


## ARMS

**Algorithm Adaptive rejection Metropolis sampling (ARMS).**

$(2)$ WHILE {{< katex >}}t\leq T{{< /katex >}}:

(a)Build a proposal function, {{< katex >}}q_m(\theta){{< /katex >}}, given a set of support points {{< katex >}}S^{(m)}=\left\{\theta_1^{(m)},\theta_2^{(m)},\cdots,\theta_{K_0}^{(m)}\right\}{{< /katex >}}, according to

{{< katex display=true >}}
W_m(\theta)=\left\{
    \begin{array}{ccc}
      L_{1,2}^{(m)}(\theta) &  & \theta\in\mathcal{I}_0^{(m)}  \\
      max\left\{L_{1,2}^{(m)}(\theta),L_{2,3}^{(m)}(\theta)\right\} &  & \theta\in\mathcal{I}_1^{(m)} \\
      \phi_j^{(m)}(\theta) &  & \theta\in\mathcal{I}_j \\
      max\left\{L_{K_m-2,K_m-1}^{(m)}(\theta),L_{K_m-1,K_m}^{(m)}(\theta)\right\} &  & \theta\in\mathcal{I}_{K_m-1}^{(m)} \\
      L_{K_m-1,K_m}^{(m)}(\theta) &  & \theta\in\mathcal{I}_{K_m}^{(m)}
    \end{array} \right.
{{< /katex >}}
(b) Draw {{< katex >}}\theta'\sim \bar{q}_m(\theta){{< /katex >}} and {{< katex >}}u\sim U([0,1)){{< /katex >}}.

(c) If {{< katex >}}u> \frac{\pi(\theta')}{q_m(\theta')}{{< /katex >}}, then reject {{< katex >}}\theta'{{< /katex >}}, update {{< katex >}}S^{m+1}=S^{m} \cup \left\{ \theta' \right\},K_{m+1}=K_m+1{{< /katex >}}, and set {{< katex >}}m=m+1{{< /katex >}}. Go back to step 2(a).

(d) Otherwise(i.e., if {{< katex >}}u \leq \pi(\theta')/q_m(\theta') {{< /katex >}}), draw {{< katex >}}u'\sim U([0,1)) {{< /katex >}} and compute the acceptance probability:

{{< katex display=true >}}
\alpha_t\equiv \alpha(\theta',\theta^{(t-1)})=min[1,\frac{\pi(\theta')min[\pi(\theta^{(t-1)}),q_m(\theta^{(t-1)})]}{\pi(\theta^{(t-1)})min[\pi(\theta'),q_m(\theta')]}]
{{< /katex >}}

(e) If {{< katex >}}u'\leq \alpha_t{{< /katex >}}, accept {{< katex >}}\theta'{{< /katex >}} and set {{< katex >}}\theta^{(t)}=\theta'{{< /katex >}}. Otherwise (i.e., if {{< katex >}}u'<\alpha_t{{< /katex >}}), reject {{< katex >}}\theta'{{< /katex >}} and set {{< katex >}}\theta^{(t)}=\theta^{(t-1)}{{< /katex >}}.

(f) Set {{< katex >}}S^{(m+1)}=S^{(m)},K_{m+1}=K_m,m=m+1,t=t+1{{< /katex >}} and return to step 2(a).

$(3)$ Approximate the integral
{{< katex display=true >}}
\hat{I}_{(T-T_b)}=\frac{1}{T-T_b}\sum_{t=T_b+1}^{T}g(\theta^{(t)})
{{< /katex >}}


## A{{< katex >}}^2{{< /katex >}}RMS

Two alternative strategies are raised to improve the standard ARMS algorithm. We denote these two variants as A{{< katex >}}^2{{< /katex >}}RMS and IA{{< katex >}}^2{{< /katex >}}RMS where the A{{< katex >}}^2{{< /katex >}} emphasizes that we incorporate an additional adaptive step to improve the proposal density w.r.t. the standard ARMS.

**Algorithm Doubly adaptive rejection Metropolis sampling (A{{< katex >}}^2{{< /katex >}}RMS).**

$(1)$ Initialization: Set {{< katex >}}t=0{{< /katex >}} (chain’s iteration) and {{< katex >}}m=0{{< /katex >}} (algorithm’s iteration). Choose an initial state {{< katex >}}(\theta^{(0)}){{< /katex >}}, an initial number of support points {{< katex >}}(K_0){{< /katex >}}, an initial support set {{< katex >}}S(0)=\left\{\theta_1^{(0)},\cdots,\theta_{K_0}^{(0)}\right\}{{< /katex >}}, the number of additional adaptive step $T_l$, the total number of iterations {{< katex >}}(T){{< /katex >}} and the burn-in period {{< katex >}}(T_b){{< /katex >}}.

$(2)$ WHILE {{< katex >}}t\leq T{{< /katex >}}:

(a) Build a proposal function, {{< katex >}}q_m(\theta) {{< /katex >}}, given a set of support points {{< katex >}}S^{(m)}=\left\{\theta_1^{(m)},\theta_2^{(m)},\cdots,\theta_{K_0}^{(m)}\right\}{{< /katex >}}, use some simple non-parametric approach.

(b) Draw {{< katex >}}\theta'\sim \bar{q}_m(\theta){{< /katex >}} and {{< katex >}}u\sim U([0,1)){{< /katex >}}.

(c) If {{< katex >}}u> \frac{\pi(\theta')}{q_m(\theta')}{{< /katex >}}, then reject $\theta'$, update {{< katex >}}S^{m+1}=S^{m} \cup \left\{ \theta' \right\},K_{m+1}=K_m+1{{< /katex >}}, and set {{< katex >}}m=m+1{{< /katex >}}. Go back to step 2(a).

(d) Otherwise(i.e., if {{< katex >}}u \leq \pi(\theta')/q_m(\theta'){{< /katex >}}), draw {{< katex >}}u'\sim U([0,1)){{< /katex >}} and compute the acceptance probability:

{{< katex display=true >}}
\alpha_t\equiv \alpha(\theta',\theta^{(t-1)})=min[1,\frac{\pi(\theta')min[\pi(\theta^{(t-1)}),q_m(\theta^{(t-1)})]}{\pi(\theta^{(t-1)})min[\pi(\theta'),q_m(\theta')]}] 
{{< /katex >}}


(e)If {{< katex >}}m\leq T_l{{< /katex >}}, draw {{< katex >}}u''\sim U([0,1]){{< /katex >}}, if {{< katex >}}u''>q_m(\theta')/\pi(\theta'){{< /katex >}}, set {{< katex >}}S^{(m+1)}=S^{(m)}\cup \mathcal{\theta'}{{< /katex >}} and {{< katex >}}K_{m+1}=K_m+1{{< /katex >}} and sort {{< katex >}}S_{t+1}{{< /katex >}} in ascending order. Otherwise, set {{< katex >}}S^{(m+1)}=S^{(m)}{{< /katex >}} and {{< katex >}}K_{m+1}=K_m{{< /katex >}}.

$(3)$ Approximate the integral: 
{{< katex display=true >}}
\hat{I}_{(T-T_b)}=\frac{1}{T-T_b} \sum_{t=T_b+1}^{T} g(\theta^{(t)})
{{< /katex >}}

## IA{{< katex >}}^2{{< /katex >}}RMS

**Algorithm Independent doubly adaptive rejection Metropolis sampling (IA{{< katex >}}^2{{< /katex >}}RMS).**

$(1)$ Initialization: Set {{< katex >}}t=0{{< /katex >}} (chain’s iteration) and {{< katex >}}m=0{{< /katex >}} (algorithm’s iteration). Choose an initial state {{< katex >}}(\theta^{(0)}){{< /katex >}}, an initial number of support points {{< katex >}}(K_0){{< /katex >}}, an initial support set {{< katex >}}S(0)=\left\{\theta_1^{(0)},\cdots,\theta_{K_0}^{(0)}\right\}{{< /katex >}}, the total number of iterations {{< katex >}}(T){{< /katex >}} and the burn-in period {{< katex >}}(T_b){{< /katex >}}.

$(2)$ WHILE {{< katex >}}t\leq T{{< /katex >}}:

(a) Build a proposal function, {{< katex >}}q_m(\theta){{< /katex >}}, given a set of support points {{< katex >}}S^{(m)}=\left\{\theta_1^{(m)},\theta_2^{(m)},\cdots,\theta_{K_0}^{(m)}\right\}{{< /katex >}}, use some simple non-parametric approach.

(b) Draw {{< katex >}}\theta'\sim \bar{q}_m(\theta){{< /katex >}} and {{< katex >}}u\sim U([0,1)){{< /katex >}}.

(c) If {{< katex >}}u> \frac{\pi(\theta')}{q_m(\theta')}{{< /katex >}}, then reject {{< katex >}}\theta'{{< /katex >}}, update {{< katex >}}S^{m+1}=S^{m} \cup \left\{ \theta' \right\},K_{m+1}=K_m+1{{< /katex >}}, and set {{< katex >}}m=m+1{{< /katex >}}. Go back to step 2(a).

(d) Otherwise(i.e., if {{< katex >}}u \leq \pi(\theta')/q_m(\theta'){{< /katex >}}), draw {{< katex >}}u'\sim U([0,1)) {{< /katex >}} and compute the acceptance probability:

{{< katex display=true >}}
\alpha_t\equiv \alpha(\theta',\theta^{(t-1)})=min[1,\frac{\pi(\theta')min[\pi(\theta^{(t-1)}),q_m(\theta^{(t-1)})]}{\pi(\theta^{(t-1)})min[\pi(\theta'),q_m(\theta')]}]
{{< /katex >}}


(e) If {{< katex >}}u'\leq \alpha_t{{< /katex >}}, accept {{< katex >}}\theta'{{< /katex >}} and set {{< katex >}}\theta^{(t)}=\theta'{{< /katex >}}. Otherwise (i.e., if {{< katex >}}u'<\alpha_t{{< /katex >}}), reject {{< katex >}}\theta'{{< /katex >}} and set {{< katex >}}\theta^{(t)}=\theta^{(t-1)}{{< /katex >}} and {{< katex >}}\vartheta=\theta'{{< /katex >}}.

(f) Draw {{< katex >}}u''\sim U([0,1)){{< /katex >}}. If {{< katex >}}u''>q_m(\vartheta)/\pi(\vartheta){{< /katex >}}, set {{< katex >}}S^{(m+1)}=S^{(m)}\cup \left\{\vartheta\right\}{{< /katex >}} and {{< katex >}}K_{m+1}=K_m+1{{< /katex >}}. Otherwise, (i.e., if {{< katex >}}u''\leq q_m(\vartheta)/\pi(\vartheta){{< /katex >}}), set {{< katex >}}S^{(m+1)}=S^{(m)} {{< /katex >}} and {{< katex >}}K_{m+1}=K_m+1{{< /katex >}}.

(g) Set {{< katex >}}m=m+1,t=t+1{{< /katex >}} and return to step 2(a).

$(3)$ Approximate the integral:
{{< katex display=true >}}
\hat{I}_{(T-T_b)}=\frac{1}{T-T_b} \sum_{t=T_b+1}^{T} g(\theta^{(t)})
{{< /katex >}}




