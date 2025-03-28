# Maximum Likelihood Estimator 2
- Last lecture introduced principle of MLE for general statistical inference
- Today discuss MLE's favorable consistency property
	- See relationship to information theory concepts
- See how MLE applies more specifically to machine learning context
- Start discussion with linear regression from MLE perspective
# MLE in the limit of infinite data
- What happens as number of data points $N \to \infty$
	- $$\huge \begin{align}
\underset{ \theta }{ \arg \max } \sum_{i}^N \log P_{\theta}(x_{i}) &= \underset{ \theta }{ \arg \max } \frac{1}{N}\sum_{i}^N \log P_{\theta}(x_{i}) \\ \\
&\overset{ N \to \infty }{ \to } = \underset{ \theta }{ \arg \max } \;\mathbb{E}_{P_{\hat{\theta}}}[\log P_{\theta}(X)]
\end{align}$$
	- As $N \to infty$, this arg max turns into an expected value of the log probability of the distribution of $X$
- Remember that, generally speaking, this is what motivates [Monte Carlo estimation](https://jwmi.github.io/BMS/chapter5-monte-carlo.pdf)
	- $$\huge \begin{align}
		\mathbb{E}_{P}[f(X)] \approx \frac{1}{N} \sum_{i = 1}^N f(x_{i})
		\end{align}$$
	- $x_{1}, \dots, x_{n} \overset{ i.i.d }{ \sim } P$
	- $P$ is a distribution over all possible outputs of $f(X)$
	- The Monte Carlo estimation (AKA Monte Carlo method) approximates the expected value of a distribution by taking samples from the distribution
		- The approximation gets more accurate with a larger $N$
- An attractive property of MLE is that is **consistent**
	- Given large enough $N$, if $\hat{\theta} \in \Theta$, then $\theta_{MLE}$ will eventually equal $\hat{\theta}$
	- $\theta_{MLE}$ will eventually recover $\hat{\theta}$, the actual parameters that represent the data distribution
# MLE and information theory
- Recall definition of cross-entropy
	- $$\huge H(P, Q) = \mathbb{E}_{p} [-\log Q(X)]$$
- Let's plug in $p_{\hat{\theta}}$ (the true data distribution) for $p$, and some $p_{\theta}$ for $q$
- $$\huge H(P_{\hat{\theta}}, P_{\theta}) =\mathbb{E}_{P_{\hat{\theta}}} [-\log P_{\theta}(X)] \approx \frac{1}{N} \sum_{i = 1}^N - \log P_{\theta} (x_{i})$$
- Maximizing log likelihood is approximately
	- Minimizing cross-entropy
	- Minimizing this KL divergence
	- $$\huge D_{KL} (P_{\hat{\theta}} \parallel P_{\theta}) = H(P_{\hat{\theta}}, P_{\theta}) - H(P_{\hat{\theta}})$$
		- Note that $H(P_{\hat{\theta}})$ is a constant with respect to $\theta$, which we actually change
		- Therefore we're left with 
			- $D_{KL}(P_{\hat{\theta}} \parallel P_{\theta}) = H(P_{\hat{\theta}}, P_{\theta})$