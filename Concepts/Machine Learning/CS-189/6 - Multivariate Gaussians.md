# 6 Multivariate Gaussians
- Multivariate Gaussians (MVGs or MVNs for multivariate normals)
	- Show up a lot in machine learning
- Why?
	- Tractability -> sometimes are useful for simplifying analyses
	- Ubiquity -> MVGs accurately describe many complex phenomena thanks to central limit theorem (CLT)
	- Familiar with material improves mathematical maturity and understanding of concepts such as **covariance**
# MVGs: the definition
- Recall univariate Gaussian distribution
	- $$\huge \mathcal{N}(x_{i}; \mu, \sigma^2) = \frac{1}{\sqrt{ 2 \pi \sigma^2 }} \exp \left\{  -\frac{1}{2} \frac{(x - \mu)^2}{\sigma^2}  \right\}$$
- Multivariate ($d$-dimensional) extension for $\mathbf{x} \in \mathbb{R}^d$
	- $$\huge \mathcal{N}(\mathbf{x}; \mu, \Sigma) = \frac{1}{\sqrt{ | 2 \pi \Sigma| }} \exp \left\{  -\frac{1}{2} (\mathbf{x} - \mu)^T \Sigma^{-1} (\mathbf{x} - \mu)  \right\}$$
		- $\mathbf{x} =$ vector of inputs
		- $\mu =$ vector of means
		- $\Sigma =$ Covariance matrix
			- Also a PSD
# Covariance and level sets
- The covariance matrix $\Sigma$ contains covariances (variances along the diagonal)
	- $\Sigma_{ij} = \text{cov}(X_{i}, X_{j}) = \mathbb{E}[(X_{i} - \mathbb{E}[X_{i}])(X_{j} - \mathbb{E}[X_{j}])]$
- For Gaussian RVs (random variables) $X_{i}, X_{j}$, the two variables are $X_{i} \perp X_{i}$ (independent) if, and only if, $\text{cov}(X_{i}, X_{j}) = 0$
- When we visualize/draw two-dimensional MVGs, we will often draw **level sets** $\{ (x_{1}, x_{2}) : p(x_{1}, x_{2}) = c \}$
	- How do we visualize higher dimensions? We don't, really...
	- For MVGs, level sets are ellipsoids
		- $(\mathbf{x} - \mu)^T \Sigma^{-1}(\mathbf{x} - \mu)= c'$
		- 