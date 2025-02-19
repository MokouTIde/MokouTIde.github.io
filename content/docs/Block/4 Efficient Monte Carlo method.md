## Single-node MCMC

The detection of modules consists in inferring the most likely model parameters which generated the observed network.

One does this by finding the best partition {{< katex >}}\left\{b_i\right\}{{< /katex >}} of the nodes, where {{< katex >}}b_i\in [1,B]{{< /katex >}} is the block membership of node {{< katex >}}i{{< /katex >}}, in the observed network {{< katex >}}G{{< /katex >}}, which maximizes the posterior likelihood {{< katex >}}\mathcal{P}(G|\left\{b_i\right\}){{< /katex >}}.

Because each graph with the same edge counts {{< katex >}}e_{rs}{{< /katex >}} are equally likely, the posterior likelihood is {{< katex >}}\mathcal{P}(G|\left\{b_i\right\})=1/\Omega(\left\{e_{es}\right\},\left\{n_r\right\}){{< /katex >}}, where {{< katex >}}e_{rs}{{< /katex >}} and {{< katex >}}n_r{{< /katex >}} are the edge and node counts associated with the block partition {{< katex >}}\left\{b_i\right\}{{< /katex >}}, and {{< katex >}}\Omega(\left\{e_{es}\right\},\left\{n_r\right\}){{< /katex >}} is the number of different network realizations.

Hence, maximizing the likelihood is identical to minimizing the microcanonical entropy {{< katex >}}S(\left\{e_{es}\right\},\left\{n_r\right\})=ln \Omega(\left\{e_{es}\right\},\left\{n_r\right\}){{< /katex >}}, which can be computed as:
traditional model

{{< katex display=true >}}
S_t=\frac{1}{2}\sum_{rs}^{}n_r n_s H_b(\frac{e_{rs}}{n_r n_s})
{{< /katex >}}

degree corrected variant

{{< katex display=true >}}
  S_c\simeq -E-\sum_{k}^{}N_k\ln{k!}-\frac{1}{2}\sum_{rs}^{}e_{rs}\ln(\frac{e_{rs}}{e_r e_s})
{{< /katex >}}

where {{< katex >}}E=\sum_{rs}^{}e_{rs}/2{{< /katex >}} is the total number of edges, {{< katex >}}N_k{{< /katex >}} is the total number of nodes with degree {{< katex >}}k{{< /katex >}}, {{< katex >}}e_r=\sum_{s}^{}e_{rs}{{< /katex >}} is the number of half-edges incident on block {{< katex >}}r{{< /katex >}}, and {{< katex >}}H_b(x)=-x\ln{x}-(1-x)\ln(1-x){{< /katex >}} is the binary entropy function and it was assumed that {{< katex >}}n_r\gg 1{{< /katex >}}.

Although minimizing {{< katex >}}S_{t/c}{{< /katex >}} allows one to find the most likely partition into {{< katex >}}B{{< /katex >}} blocks, it cannot be used to find the best value of {{< katex >}}B{{< /katex >}} itself. This is because the minimum of {{< katex >}}S_{t/c}{{< /katex >}} is a strictly decreasing function of {{< katex >}}B{{< /katex >}}, since larger models can always incorporate more details of the observed data, providing a better fit.

Indeed, if one minimizes {{< katex >}}S_{t/c}{{< /katex >}} over all {{< katex >}}B{{< /katex >}} values one will always obtain the trivial {{< katex >}}B=N{{< /katex >}} partition where each node is in its own block, which is not a useful result. The task of identifying the best value of {{< katex >}}B{{< /katex >}} in a principled fashion is known as model selection, which attempts to separate actual structure from noise and avoids overfitting.

The simplest approach one can take is to attempt to move each vertex into one of the {{< katex >}}B{{< /katex >}} blocks with equal probability. This easily satisfies the requirements of ergodicity and detailed balance but can be very inefficient.\\
This is particularly so in the case where the value of {{< katex >}}B{{< /katex >}} is large and the block structure of the network is well defined, such that the vertex will belong to very few of the {{< katex >}}B{{< /katex >}} blocks with a nonvanishing probability, which means that most random moves will simply be rejected.

A better approach has been proposed\footfullcite{Tiago P. Peixoto, Parsimonious Module Inference in Large Networks. Phys. Rev. Lett. 110, 169905 (2013)}, which we present here in a slightly generalized fashion and consists in attempting tomove a vertex from block {{< katex >}}r{{< /katex >}} to {{< katex >}}s{{< /katex >}} with a probability given by

{{< katex display=true >}}
   p(r\rightarrow s|t)=\frac{e_{ts}+\epsilon}{e_t+\epsilon B}
 {{< /katex >}}

where {{< katex >}}t{{< /katex >}} is the block label of a randomly chosen neighbor and {{< katex >}}\epsilon>0{{< /katex >}} is a free parameter (note that by making {{< katex >}}\epsilon\rightarrow\infty{{< /katex >}} we recover the fully random moves described above).

Fig.singlenode1

It should be observed that this move imposes no inherent bias; in particular, it does not attempt to find assortative structures in preference to any other, since it depends fully on the matrix {{< katex >}}e_{rs}{{< /katex >}} currently inferred.

For any choice of {{< katex >}}\epsilon>0{{< /katex >}}, this move proposal fulfills the ergodicity condition but not detailed balance. However, this can be enforced in the usual Metropolis-Hastings fashion by accepting each move with a probability a given by

{{< katex display=true >}}
a=min\left\{e^{-\beta\Delta S_{t/c}}\frac{\sum_{t}^{}p^i_t p(s\rightarrow r|t)}{\sum_{t}^{}p^i_t p(r\rightarrow s|t)},1 \right\}
{{< /katex >}}

where {{< katex >}}p^i_t{{< /katex >}} is the fraction of neighbors of node {{< katex >}}i{{< /katex >}} which belong to block {{< katex >}}t{{< /katex >}}, and {{< katex >}}p(r\rightarrow s|t){{< /katex >}} is computed after the proposed {{< katex >}}r\rightarrow s{{< /katex >}} move (i.e., with the new values of {{< katex >}}e_{rt}{{< /katex >}}), whereas {{< katex >}}p(r\rightarrow s|t){{< /katex >}} is computed before. The parameter {{< katex >}}\beta{{< /katex >}} is an inverse temperature, which can be used to escape local minima or to turn the algorithm into a greedy heuristic.

Namely, the mixing time can be heavily dependent on how close one starts from the typical partitions which are obtained after equilibration. Since one does not know this, one often starts with a random partition.

However, this is very far from the equilibrium states, and if the block structure is sufficiently strong, this can lead to metastable configurations, where the block structure is only partially discovered, as shown in figure.

Fig.singlenode2.png

Evolution of the MCMC for a network sampled from the PP model with {{< katex >}}N=10^4{{< /katex >}}, {{< katex >}}<k>=10{{< /katex >}}, {{< katex >}}B=3{{< /katex >}}, and {{< katex >}}c=0.99{{< /katex >}}, starting from a fully random partition of the nodes. The networks show a representative snapshot of the state of the system before and after the last drop in {{< katex >}}S_t{{< /katex >}}.

### Agglomerative heuristic algorithm
In order to avoid the metastable states described previously, we explore the fact that they are more likely to occur if the block sizes are large, since otherwise the quenched topological fluctuations present in the network will offer a smaller free-energy barrier which needs to be overcome. Therefore, a more promising approach is to attempt to find the best configuration for some {{< katex >}}B'>B{{< /katex >}} and then use that configuration to obtain a better estimate for one with {{< katex >}}B{{< /katex >}} blocks.

We implement this by constructing a block (multi-)graph, where the blocks themselves are the nodes (weighted by the block sizes) and the edge counts {{< katex >}}e_{rs}{{< /katex >}} are the edge multiplicities between each block node. In this representation, a block merge is simply a block membership move of a block node, where initially each node is in its own block.

Fig.singlenode3.png

In order to select the best merges, we attempt {{< katex >}}n_m{{< /katex >}} moves for each block node and collectively rank the best moves for all nodes according to {{< katex >}}\Delta S_{t/c}{{< /katex >}}. From this global ranking, we select the best {{< katex >}}B'-B{{< /katex >}} merges to obtain the desired partition into {{< katex >}}B{{< /katex >}} blocks.

However, if the value of {{< katex >}}N/B'{{< /katex >}} itself is too large, we face again the same problem as before. Therefore we proceed iteratively by starting with {{< katex >}}B_1=N{{< /katex >}}, and selecting {{< katex >}}B_{i+1}=B_i/\sigma{{< /katex >}}, until we reach the desired {{< katex >}}B{{< /katex >}} value, where {{< katex >}}\sigma>1{{< /katex >}} controls how greedily the merges are performed.

To diminish the effect of bad merges done in the earlier steps, we also allow individual node moves between each merge step, by applying the MCMC steps above to the original network, with {{< katex >}}\beta\rightarrow\infty{{< /katex >}}. The complexity of each agglomerative step is {{< katex >}}O[n_mE+N\ln(B_i-B_{i-1})+\tau E]{{< /katex >}}, which incorporates the search for the merge candidates, the ranking of the {{< katex >}}B_i-B_{i-1}{{< /katex >}} best merges, and the movement of the individual nodes, where {{< katex >}}\tau{{< /katex >}} is the necessary amount of sweeps to reach a local minimum.

Hence, we have an overall complexity of {{< katex >}}O\left\{[(n_m+\tau)E+N\ln{N}]\times\ln{N}/\ln\sigma\right\}\sim O(N\ln^2N){{< /katex >}}, if we assume that {{< katex >}}B\ll N^4{{< /katex >}} and that the graph is sparse with {{< katex >}}E\sim O(N){{< /katex >}}.

This approach is capable of almost always avoiding the metastable configurations described previously.

Fig.singlenode4.png

Left: An example of a typical partition obtained by starting with a random {{< katex >}}B=3{{< /katex >}} configuration and applying only greedy moves until no further improvement is possible for a PP network with {{< katex >}}N=300{{< /katex >}}, {{< katex >}}<k>=10{{< /katex >}}, and {{< katex >}}c=0.9{{< /katex >}}. Right: A typical outcome for the same network, with the greedy agglomerative algorithm described in the text.

result fig*2

Normalized mutual information (NMI) between the planted and the inferred partitions for (left) the PP model and (right) the circular multipartite model described in the text, as a function of the modular strength {{< katex >}}c{{< /katex >}}, for {{< katex >}}N=10^4{{< /katex >}} and {{< katex >}}B=10{{< /katex >}}.


## Merge-Split MCMC

The inference approach to community detection involves first the stipulation of a generative model for the network structure given a network partition {{< katex >}}b=\left\{b_1,\cdots,b_N\right\}{{< /katex >}}, where {{< katex >}}b_i\in[1,B]{{< /katex >}} is the membership of node {{< katex >}}i{{< /katex >}} in one of {{< katex >}}B{{< /katex >}} groups. For example, when using the degree-corrected stochastic block model (DC-SBM)\footfullcite{A. Decelle, F. Krzakala, C. Moore, and L. Zdeborová, Asymptotic analysis of the stochastic block model for modular networks and its algorithmic applications, Phys. Rev. E 84, 066106 (2011).}, it is assumed that a network with adjacency matrix {{< katex >}}A{{< /katex >}} is generated with probability

 {{< katex display=true >}}
   P(A|\lambda,\theta,b)=\prod_{i<j}^{}\frac{(\lambda_{b_i b_j}\theta_i\theta_j)^{A_{ij}}e^{-\lambda_{b_i b_j}\theta_i\theta_j}}{A_{ij}!}\times
   \prod_{i}^{}\frac{(\lambda_{b_i b_j}\theta_i^2/2)^{A_{ij}/2}e^{-\lambda_{b_i b_j}\theta_i^2/2}}{(A_{ij}/2)!}
 {{< /katex >}}
 
with the additional parameters {{< katex >}}\lambda{{< /katex >}} and {{< katex >}}\theta{{< /katex >}} specifying the affinity between groups and expected node degrees, respectively.

The inference procedure consists in sampling from the posterior distribution

 {{< katex display=true >}}
   P(b|A)=\frac{P(A|b)P(b)}{P(A)}
 {{< /katex >}}
 
where {{< katex >}}P(b){{< /katex >}} is a prior probability of node partitions, and

 {{< katex display=true >}}
   P(A|b)=\int_{}^{}P(A|\lambda,\theta,b)P(\theta|b)P(\lambda|b)d\theta d\lambda
 {{< /katex >}}
 
is the marginal likelihood integrated over the remaining model parameters, weighted according to their own prior probabilities.

If we make a noninformative choice for them,

 {{< katex display=true >}}
   P(\theta|b)=\prod_{i}^{}(n_r-1)!\delta(\sum_{i}^{}\theta_i\delta_{b_i,r}-1)
 {{< /katex >}}
 
 {{< katex display=true >}}
   P(\lambda|b,\bar{\lambda})=\prod_{r<s}^{}e^{-\lambda_{rs}/\bar{\lambda}}/\bar{\lambda}\prod_{r}^{}e^{-\lambda_{rs}/2\bar{\lambda}}/2\bar{\lambda}
 {{< /katex >}}
 
then we can compute the integral exactly as

 {{< katex display=true >}}
   P(A|b)=\frac{\bar{\lambda}^E}{(\bar{\lambda}+1)^{E+B(B+1)/2}}\times\frac{\prod_{r<s}^{}e_{rs}!\prod_{r}^{}e_{rr}!}{\prod_{i<j}^{}A_{ij}!\prod_{i}^{}A_{ii}!}
   \prod_{r}^{}\frac{(n_r-1)!}{(e_r+n_r-1)!}\prod_{i}^{}k_i!
 {{< /katex >}}
 
where {{< katex >}}e_{rs}=\sum_{ij}^{}A_{ij}\delta_{b_i,r}\delta_{b_j,s}{{< /katex >}}, {{< katex >}}e_r=\sum_{s}^{}e_{rs}{{< /katex >}}, and {{< katex >}}n_r=\sum_{i}^{}\delta_{b_i,r}{{< /katex >}}.

The last remaining quantity,

 {{< katex display=true >}}
   P(A)=\sum_{b}^{}P(A|b)P(b)
 {{< /katex >}}
 
is called the evidence and serves as a normalization constant for the posterior distribution.

The simplest kind of move proposal that can be done is the one that involves the change of a single node {{< katex >}}i{{< /katex >}} from its current group {{< katex >}}b_i=r{{< /katex >}} to another group {{< katex >}}s{{< /katex >}}, according to a local move proposal {{< katex >}}P(s|i,b){{< /katex >}}, leading to

 {{< katex display=true >}}
   P(b'|b)=\sum_{i}^{}P(i)\prod_{j}^{}[P(b'_j|j,b)]^{\delta_{ij}}[\delta_{b_i',b_i}]^{1-\delta_{ij}}
 {{< /katex >}}
 
with {{< katex >}}P(i){{< /katex >}} being the probability of choosing node {{< katex >}}i{{< /katex >}} to perform such a move.

Note that we do not need to compute this whole expression when calculating the MH acceptance criterion, as the forward and reverse proposal must involve the same node, which also means that the probability {{< katex >}}P(i){{< /katex >}} will cancel out, and hence we are allowed to choose nodes with arbitrary frequencies, as long as they are nonzero to guarantee ergodicity.

We choose to move to existing groups with a probability given by
 {{< katex display=true >}}
   P_e(s|i,b)=\sum_{t}^{}w_{it}\frac{e_{ts}+\epsilon}{e_t+\epsilon B(b)}
 {{< /katex >}}
 
where {{< katex >}}w_{it}=\sum_{j}^{}A_{ij}\delta_{b_j,t}/k_i{{< /katex >}} is the fraction of neighbors of node {{< katex >}}i{{< /katex >}} that belong to group {{< katex >}}t{{< /katex >}}. The constant {{< katex >}}\epsilon{{< /katex >}} must be nonzero to guarantee ergodicity (a reasonable choice is simply {{< katex >}}\epsilon=1{{< /katex >}}).

Moreover, we consider the situation of the occupation of a new group. Hence we incorporate this kind of move by augmenting our proposals as
 {{< katex display=true >}}
   P(s|i,b)=\left\{
    \begin{array}{lll}
      d &  & \text{if {{< katex >}}s{{< /katex >}} is a new group} \\
      (1-d)P_e(s|i,b) &  & \text{otherwise}
    \end{array}
    \right.
 {{< /katex >}}
 
where {{< katex >}}d{{< /katex >}} is the probability with which the population of a new group is attempted.

To overcome the limitations of single-node moves, we may introduce moves where entire groups are merged together. We implement this by selecting an existing group {{< katex >}}r{{< /katex >}} with a uniform probability {{< katex >}}P(r)=1/B(b){{< /katex >}}  and moving all its nodes to a different group {{< katex >}}s{{< /katex >}} with probability {{< katex >}}P(s|r,b){{< /katex >}}, yielding a transition proposal

 {{< katex display=true >}}
   P(b'|b)=\sum_{rs}^{}P(r)P(s|r,b)[\prod_{i}^{}(\delta_{b_i',s})^{\delta_{b_i,r}}(\delta_{b_i',bi})^{1-\delta_{b_i,r}}]
 {{< /katex >}}
 
Following the same line as before, we choose a smarter merge proposal rather than choose the proposal uniformly as random is

 {{< katex display=true >}}
   P(s|r,b)=\frac{1-\delta_{rs}}{n_r}\sum_{i}^{}\delta_{b_i,r}\frac{P_e(s|i,b)}{1-P_e(r|i,b)}
 {{< /katex >}}
 
where {{< katex >}}P_e(s|i,b)=\sum_{t}^{}w_{it}\frac{e_{ts}+\epsilon}{e_t+\epsilon B(b)}{{< /katex >}}

Such merge proposals are straightforward to implement, but in order for this to incorporated into the MH scheme we need to be able to propose “split” moves that go in the reverse direction, i.e., we need to be able to select a subset of the nodes in group {{< katex >}}r{{< /katex >}} and move them to a new group {{< katex >}}s{{< /katex >}}.

We denote this subset via a binary vector {{< katex >}}x=\left\{0,1\right\}^N{{< /katex >}}, where {{< katex >}}x_i=1{{< /katex >}} if node {{< katex >}}i{{< /katex >}} is selected to move from group {{< katex >}}r{{< /katex >}} to {{< katex >}}s{{< /katex >}}; otherwise {{< katex >}}x_i=0{{< /katex >}}. This leads us to a move proposal that can be written as

 {{< katex display=true >}}
   P(b'|b)=\sum_{r}^{}P(r)\sum_{x}^{}P(x|r,b)[\prod_{i}^{}(\delta_{b_i',s(b)})^{\delta_{x_i,1}}(\delta_{b_i',b_i})^{\delta_{x_i,0}}]
 {{< /katex >}}
 
where {{< katex >}}s(b){{< /katex >}} denotes the new group not currently occupied in partition {{< katex >}}b{{< /katex >}} (we do not differentiate between unoccupied groups).

A simple, but ultimately naive way of moving forward is to sample {{< katex >}}x{{< /katex >}} in two stages: at first we select the number {{< katex >}}m{{< /katex >}} of nodes to be moved uniformly at random in the interval {{< katex >}}[1,n_r-1]{{< /katex >}} with probability

 {{< katex display=true >}}
   P(m|r,b)=\frac{1}{n_r-1}
 {{< /katex >}}
 
and then sample the m nodes uniformly at random, with a probability
 {{< katex display=true >}}
   P(x|r,b,m)=\frac{\delta_{m,\sum_{i}^{}x_i}}{\binom{n_r}{m}}
 {{< /katex >}}

Sample the {{< katex >}}m{{< /katex >}} nodes uniformly at random, with a probability
 {{< katex display=true >}}
   P(x|r,b,m)=\frac{\delta_{m,\sum_{i}^{}x_i}}{\binom{n_r}{m}}
 {{< /katex >}}
 
yielding thus
 {{< katex display=true >}}
   P(x|r,b)=\sum_{m=1}^{n_r-1}P(x|r,b,m)P(m|r,b)
 {{< /katex >}}
 
Although this approach is easy to implement, it does not work in practice, because such fully random split proposals are almost never accepted, since the number of good splits, even when they exist, is exponentially outnumbered by bad ones.

Furthermore, the above puts a low probability for every possible split, which means that it will also cause good merges to be rejected, as they cannot be easily reversed under this scheme.

### Auxiliary variables and split staging
Recall the detailed balance condition that needs to be fulfilled for the Markov chain to converge to the target distribution
 {{< katex display=true >}}
   \pi(b)T(b'|b)=\pi(b')T(b|b')
 {{< /katex >}}
 
where {{< katex >}}T(b'|b){{< /katex >}} is the transition probability, after the rejection step has been considered.

Now, let us consider an augmented version of the posterior distribution obtained by sampling an arbitrary auxiliary variable {{< katex >}}α{{< /katex >}} with probability {{< katex >}}P(\alpha,b){{< /katex >}}. If we use MCMC to sample from the joint distribution {{< katex >}}P(\alpha,b)=P(\alpha|b)\pi(b){{< /katex >}}, then we can marginalize it to obtain the original distribution {{< katex >}}\pi(b)=\sum_{\alpha}^{}P(\alpha,b){{< /katex >}}, therefore sampling from this augmented space subsumes sampling from the original one.

The usefulness of introducing this auxiliary variable comes from the fact that we can use it to condition our move proposals, as we will see. The detailed balance condition in this case reads
 {{< katex display=true >}}
   P(\alpha|b)\pi(b)T(\alpha',b'|\alpha,b)=P(\alpha'|b')\pi(b')T(\alpha,b|\alpha',b')
 {{< /katex >}}
 
We can choose the joint transition by first making a transition {{< katex >}}b\rightarrow b'{{< /katex >}} and then sampling the auxiliary variable from its conditional distribution, i.e.,
 {{< katex display=true >}}
   T(\alpha',b'|\alpha,b)=P(\alpha'|b')T(b'|\alpha,b)
 {{< /katex >}}
 
Then the above detailed balance condition boils down to
 {{< katex display=true >}}
   \pi(b)T(b'|b,\alpha)=\pi(b')T(b|b',\alpha')
 {{< /katex >}}
 
which, importantly, is independent of the probability {{< katex >}}P(\alpha|b){{< /katex >}}.

We can incorporate this into the Metropolis-Hastings framework by conditioning our proposals {{< katex >}}P(b'|b,\alpha){{< /katex >}}, and accepting them with probability
 {{< katex display=true >}}
   min[1,\frac{\pi(b')P(b|b',\alpha')}{\pi(b)P(b'|b,\alpha)}]
 {{< /katex >}}
which will enforce the condition above.

The key advantage here is that we do not need to know how to compute the probability {{< katex >}}P(\alpha|b){{< /katex >}}; we need only to be able to sample from this distribution.

This gives us freedom to implement more elaborate schemes, and leads us to the notion of split staging, where as an auxiliary variable we use a tentative split {{< katex >}}\hat{x}{{< /katex >}}, which is again a binary vector with elements {{< katex >}}\hat{x}_i\in\left\{0,1\right\}{{< /katex >}}, defined for the nodes that belong to the group being split, which is sampled from some arbitrary distribution (which we will discuss shortly), and we perform the final split via a single Gibbs sweep from that initial state, whose probability can be easily computed as

 {{< katex display=true >}}
   P(x|r,b,\hat{x})=\prod_{i\in V_r}^{}P(x_i|r,b,x_1,\cdots,x_{i-1},\hat{x}_{i+1},\cdots,\hat{x}_N)
 {{< /katex >}}
 
where {{< katex >}}r{{< /katex >}} is the group being split, {{< katex >}}V_r{{< /katex >}} is the set of nodes that belong to it.

Hence
 {{< katex display=true >}}
   P(x_i|r,b,x_1,\cdots,x_{i-1},\hat{x}_{i+1},\cdots,\hat{x}_N)=\frac{\pi[b'(x_1,\cdots,x_{i-1},x_i,\hat{x}_{i+1},\cdots,\hat{x}_N)]}
   {\sum_{x=1}^{1}\pi[b'(x_1,\cdots,x_{i-1},x_i,\hat{x}_{i+1},\cdots,\hat{x}_N)]}
 {{< /katex >}}
 
is the probability of moving node {{< katex >}}i{{< /katex >}} to either {{< katex >}}x_i=0{{< /katex >}} or {{< katex >}}1{{< /katex >}} in sequence, with the shortcut notation {{< katex >}}b'(x)=[b_1'(x_1),\cdots,b_N'(x_N)]{{< /katex >}} is given by
 {{< katex display=true >}}
   b_i'(x_i)=\left\{
    \begin{array}{lll}
      s &  & \text{if {{< katex >}}x_i=1{{< /katex >}} and {{< katex >}}b_i=r{{< /katex >}}} \\
      b_i &  & \text{otherwise}
    \end{array}
    \right.
 {{< /katex >}}
 
This yields a total split proposal probability

 {{< katex display=true >}}
   P(b'|b,\hat{x})=\sum_{r}^{}P(r)\sum_{x}^{}P(x|r,b,\hat{x})\prod_{i}^{}\delta_{b_i',b_i'(x_i)}
 {{< /katex >}}
 
which is conditioned on the stage split {{< katex >}}\hat{x}{{< /katex >}}, sampled from an arbitrary distribution.

As was shown before, starting from a random division, and then moving one node at a time, usually yields very bad performance, as this scheme needs a long time to find the optimal division. This happens even if the optimal division is very clear, with a very high posterior probability, since the initial random division effectively hides it from view, and the MCMC sampling essentially does a random walk in the configuration space, before it can find the best split.

As an alternative approach\footfullcite{T. P. Peixoto, Efficient Monte Carlo and greedy heuristic for the inference of stochastic block models, Phys. Rev. E 89, 012804 (2014).} that agglomerative schemes work significantly better, where one first puts each node in their own group, then proceeds by merging groups together, until the desired number of groups is reached.

What typically happens in this scheme is that the intermediary partitions reached amount to subdivisions of the final partition, thus overcoming the entropic barriers seen by the random initial division scheme, and thus achieving the good split in a shorter time.

However, we need also to consider a rather unintuitive property of the dual role of our split proposal, which functions also as a mechanism to reverse merge proposals.

After experimentation, we determined that the following scheme, which combines a variety of strategies, works well in a majority of cases. We begin by sampling a prestaging split {{< katex >}}\hat{x}^0{{< /katex >}} uniformly at random from one of the following three algorithms:

{{< katex >}}(1){{< /katex >}} Random split:

(a) We sample the split size {{< katex >}}m{{< /katex >}} uniformly at random from the interval {{< katex >}}[1, n_r-1]{{< /katex >}}.

(b) We choose {{< katex >}}\hat{x}^0{{< /katex >}} uniformly at random from all splits of size {{< katex >}}\sum_{i}^{}\hat{x}_i^0=m{{< /katex >}}.

{{< katex >}}(2){{< /katex >}} Sequential spreading:

(a) We move all nodes in group {{< katex >}}r{{< /katex >}} to an empty group {{< katex >}}t{{< /katex >}}.

(b) In random order, we chose the nodes sequentially and move them either to group {{< katex >}}r{{< /katex >}} or group {{< katex >}}s{{< /katex >}}, with probability proportional to {{< katex >}}\pi(\hat{b}_1,\cdots,\hat{b}_i,\cdots,\hat{b}_N){{< /katex >}} for a node {{< katex >}}i{{< /katex >}}, with {{< katex >}}\hat{b}{{< /katex >}} being the current working partition, except for the first two moves, which are to groups {{< katex >}}r{{< /katex >}} and {{< katex >}}s{{< /katex >}}, to prevent leaving either group empty. In the end, to a node with {{< katex >}}\hat{b}_i=r{{< /katex >}} we set {{< katex >}}\hat{x}_i^0=0{{< /katex >}}, and if {{< katex >}}\hat{b}_i=s{{< /katex >}}, we set {{< katex >}}\hat{x}_i^0=1{{< /katex >}}.

{{< katex >}}(3){{< /katex >}} Sequential coalescence:

(a) We move each node {{< katex >}}i{{< /katex >}} in group {{< katex >}}r{{< /katex >}} to its own previously empty group {{< katex >}}t_i{{< /katex >}}, which in the end all have a single node each.

(b) We proceed like {{< katex >}}2{{< /katex >}}(b) above.

From the prestage {{< katex >}}\hat{x}^0{{< /katex >}}, we obtain {{< katex >}}\hat{x}^0{{< /katex >}} by performing {{< katex >}}M{{< /katex >}} sequential Gibbs sweeps for every node in group {{< katex >}}r{{< /katex >}} and {{< katex >}}s{{< /katex >}}, where they are allowed to move only between these two groups, forbidding moves that would leave either group empty.\\
With the stage split in place, the final proposal is obtained by performing one more Gibbs sweep as described previously, and it is then accepted with probability
 {{< katex display=true >}}
   min[1,\frac{\pi(b')P(b|b')}{\pi(b)P(b'|b,\hat{x})}]
 {{< /katex >}}
where the reverse transition {{< katex >}}P(b|b'){{< /katex >}} corresponds to a merge proposal.

Likewise a merge can be accepted with probability
 {{< katex display=true >}}
   min[1,\frac{\pi(b')P(b|b',\hat{x})}{\pi(b)P(b'|b)}]
 {{< /katex >}}
which means we always need to generate a staging split {{< katex >}}\hat{x}{{< /katex >}} for each merge candidate, using the algorithm above, to compute the reverse proposal probability.

As the number {{< katex >}}M{{< /katex >}} of Gibbs sweeps made during the staging step increases, the more likely it becomes that the split will be proposed with a probability proportional to the target distribution {{< katex >}}\pi(b){{< /katex >}}, which would be optimal. In practice, however, we do not want to choose this value too large, as it is not worth to spend too much time in a single Monte Carlo step. Although the optimal value is likely to vary for each network, we found that a value around {{< katex >}}M=10{{< /katex >}} offers a good trade-off between speed and proposal quality in most experiments we made.

### Joint merge-split moves
We also consider an additional kind of move that keeps the number of groups constant, and is composed of a merge of group {{< katex >}}r{{< /katex >}} into {{< katex >}}s{{< /katex >}}, which is then split again by moving some nodes from {{< katex >}}s{{< /katex >}} to back to {{< katex >}}r{{< /katex >}}. This amounts to a proposal probability

 {{< katex display=true >}}
   P(b'|b,\hat{x})=\sum_{rs}^{}P(r)P(s|r,b)\sum_{\hat{b}}^{}[\prod_{i}^{}(\delta_{\hat{b}_i,s})^{\delta_{b_i,r}}(\delta_{\hat{b}_i,b_i})^{1-\delta_{b_i,r}}]\times \sum_{x}^{}P(x|s,\hat{b},\hat{x})\prod_{i}^{}\delta_{\hat{b}'_i(x_i),b'_i}
 {{< /katex >}}
and the final move is accepted with probability

 {{< katex display=true >}}
   min[1,\frac{\pi(b')P(b|b',\hat{x}')}{\pi(b)P(b'|b,\hat{x})}]
 {{< /katex >}}
This kind of move is able to redistribute the nodes between two groups, without going through states with a smaller or lower number of groups, which we found to improve mixing in circumstances where the number of groups tends not to vary significantly in the posterior distribution.

