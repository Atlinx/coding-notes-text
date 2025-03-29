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
# What about MLE for regression/classification?
- Given data $\mathcal{D} = \{ (\text{x}_{1}, y_{1}), \dots, (\text{x}_{N}, y_{N}) \}$
- Assume a set (family) of distributions on $(\text{x}, y)$
	- $\{ P_{\theta} : \theta \in \Theta \}$
		- The distribution answers the question, what's the probability of getting $P(\text{x}, y)$
	- $P_{\theta}(\text{x}, y) = P(\text{x})P_{\theta}(y | \text{x})$
		- $\text{x}$ comes from some fixed distribution
- Parameters $\theta$ only dictate the conditional distribution of $y$ given $\text{x}$
- Objective/definition remains the same
	- $$\large \begin{align} \\
		\underset{ \theta }{ \arg \max } \prod_{i}^N  P(\text{x}_{i})P_{\theta}(y_{i} \mid \text{x}_{i}) &= \underset{ \theta }{ \arg \max }  \sum_{i}^N [\log P(\text{x}_{i}) + \log P_{\theta}(y_{i} \mid \text{x}_{i})] \\
		&= \underset{ \theta }{ \arg \max } \sum_{i}^N \log P_{\theta}(y_{i} \mid \text{x}_{i})
		\end{align}
		$$
	-  **NOTE:** We can eliminate $\log P(\text{x}_{i})$ because it's a constant
# Example: "least squares" linear regression
- We are given data $\mathcal{D} = \{ (\text{x}_{1}, y), \dots, (\text{x}_{N}, y_{N}) \}$
	- $\text{x}$ = $d$-dimensional vector
	- $y$ = real value
- Assume that output given the input is generated i.i.d. as
	- $Y \mid X \sim \mathcal{N}(\hat{w}^TX + \hat{b}, \sigma^2)$
	- $Y = \hat{w}^TX + \hat{b} + \epsilon$, where $\epsilon \sim \mathcal{N}(0, \sigma^2)$
		- Can imagine $\epsilon$ is a noise that's added to the original linear equation
	- **NOTE:** $\hat{w}$ and variable with hats moving forward mean the true value
		- This is the opposite of traditional statistic conventions, which assign hat variables as the sample parameters, and non-hat variables as the true parameters
	- $\sigma^2$ is a true parameter value, but we don't really care about it
- We don't need to specify the input distribution since it does not depend on $\theta$
- The objective is
	- $$\huge \begin{align}
		&\underset{ \theta \in \Theta }{ \arg \max } \sum_{i}^N \log \mathcal{N}(y_{i}; w^T\text{x}_{i} + b, \sigma^2) \\ \\
		&= \underset{ \theta \in \Theta }{ \arg \max } \sum_{i}^N - \frac{1}{2\sigma^2}(w^T x_{i} + b - y_{i})^2 + C_{\text{w.r.t. } \theta}
		\end{align}$$
	- $\theta = [w, b]$
		- Parameters of our linear function, that we then set as the mean for our Gaussian distribution behind our data set
	- $\Theta =$ all possible $[w, b]$
# MLE typically requires (iterative) optimization
- For examples we have seen so far, MLE can be obtained in closed form
- We will see many examples where this is not the case
- In such cases, rely on **iterative optimization**
	- Start with guess
	- Continually refine "guess" of MLE until we are satisfied
- One can take entire series of classes on iterative optimization
