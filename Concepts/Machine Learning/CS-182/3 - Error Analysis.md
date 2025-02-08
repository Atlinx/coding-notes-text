# Error Analysis
- Will we get the right answer?
# Empirical Risk and True Risk
- Zero-one loss -> 1 if wrong, 0 if right
- Risk -> probability of getting it wrong
- $$\huge \text{Risk} = E_{x \sim p(x),y \sim p(y | x)} [\mathcal{L}(x, y, \theta)]$$
- During training, we can't sample $x \sim p(x)$, we just have $\mathcal{D}$
- Instead we can calculate empirical risk
	- Empirical -> based on observation
- $$\huge \text{Empirical risk} = \frac{1}{2} \sum_{i = 1}^n \mathcal{L}(x_{i}, y_{i}, \theta) \approx E_{x \sim p(x), y \sim p(y | x) [\mathcal{L}(x, y, \theta)]}$$
- Is this a good approximation?
# Empirical Risk Minimization
- Supervised learning is usually empirical risk minimization
- Is this the same as true risk minimization?
	- Overfitting
		- When empirical risk is low, but true risk is high
		- ![[Pasted image 20250207114744.png]]
		- Fitting a high degree polynomial
			- Low empirical risk -> passes through all points
			- True risk -> doesn't accurately model a line
		- Can happen
			- If the dataset is too small
			- If the model is too powerful (too many parameters/capacity)
	- Underfitting
		- When empirical risk is high, and true risk is high
		- ![[Pasted image 20250207114858.png]]
		- Can happen
			- If model is too weak (too few parameters/capacity)
			- If your optimizer is not configured well (wrong learn rate)
# Let's Analyze Error!
- ![[Pasted image 20250207115923.png]]
- Last time discussed classification
	- Predicting a label -> predicting the probabilities of each label
- This time, focus on regression
	- Predicting a continuous variable -> predicting a **continuous distribution** based on the variable
	- Curve fitting
		- Continuous inputs -> predict continuous output

> [!note] Terminology
> **Continuous distribution** refers to a probability distribution that describes the likelihood of all possible values of a continuous random variable.
> 
> ![[Pasted image 20250207121520.png]]

$$\huge \log p_{\theta}(y | x) = \mathcal{N}(f_{\theta}(x), \Sigma_{\theta}(x))$$
- $\log p_{\theta}(y | x)$ -> the log probability distribution we output

> [!missing] TODO
> What does $\log p_{\theta}(y|x)$ mean?

- $\mathcal{N} =$ normal (Gaussian) distribution
	- Normal distributions are defined by a mean and the variance
	- Our model wants to optimize itself to match this distribution,
		- Therefore the mean and variance are defined as functions 
- The normal distribution expands to
	- $$\large
	\begin{align}
	\log p_{\theta}(y | x) &= \mathcal{N}(f_{\theta}(x), \Sigma_{\theta}(x))  \\
	&= -\frac{1}{2} (f_{\theta}(x) - y)\Sigma_{\theta}(x)^{-1} (f_{\theta}(x) - y)) - \frac{1}{2} \log |\Sigma_{\theta}(x)| + \text{const}
	\end{align}
	$$
	- First term -> how close is the mean of theta to true label $y$
	- Second term -> tries to minimize the variance
- However, if our variance $\Sigma_{\theta}(x) = \text{I}$, the identity matrix, we can simply to
$$
\huge \begin{align}
\log p_{\theta}(y | x) &= \mathcal{N}(f_{\theta}(x), \text{I})  \\
&= - \frac{1}{2} \log ||f_{\theta}(x) - y||^2 + \text{const}
\end{align}
$$
	- Technically we can learn $\Sigma_{\theta}(x)$, but we'll use the identity matrix for simplification
- This is the same as the mean squared error (MSE) loss!
$$\huge \mathcal{L}(\theta, x, y) = -\frac{1}{2} ||f_{\theta}(x) - y ||^2$$
	- Analysis presented here will be for MSE, although we can analyze other losses as well
- Overfitting -> empirical risk is low true risk is high
- How does the error change for different training sets?
	- ![[Pasted image 20250207130554.png]]
	- Overfitting
		- Training data is fitted well
		- True function is fitted poorly
		- **Learned function looks different each time**
	- Underfitting
		- Training data is fitted poorly
		- True function is fitted poorly
		- **Learned function looks similar, even if we pool together all datasets**
- What is expected error, given a distribution over datasets?
	- ![[Pasted image 20250207131045.png]]
	- $$\huge E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\theta}(x) - y ||^2]$$
		- Expected error w.r.t (with respect to) the data distribution
		- If we repeated sample datasets of the same size, what is our expected error?
			- Note that for every dataset $\mathcal{D}$, we get a different set of parameters $\theta$
			- Let's denote $f_{\theta}(x) = f_{\mathcal{D}}(x)$
			- Let's denote $y = f(x)$
				- $f(x)$ is the true function
				- $f_{\mathcal{D}}(x)$ is the learned function
$$\huge \begin{align} &E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\theta}(x) - y ||^2] \\&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - f(x) ||^2] \\ &= \sum_{\mathcal{D}} p(\mathcal{D}) [|| f_{\mathcal{D}}(x) - f(x) ||^2] \end{align}$$
- Expected value of error -> Sum over all possible data sets' error scaled by the probability of getting each data set
- Why do we care about this quantity
	- Want to understand how well our **algorithm** does independently of the **particular (random) choice of dataset**
	- This is important if we want to improve our algorithm
# Bias-Variance Tradeoff
$$\huge \bar{f}(x) = E_{\mathcal{D} \sim p(\mathcal{D})} [f_{D}(x)]$$
- $\bar{f}(x) =$ expected value of $f_{\mathcal{D}}(x)$
	- Average over all possible functions you'd get over all data sets (weighted by probability of each data set)
$$\large \begin{align}
&E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - f(x) ||^2] \\
&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - \bar{f}(x) + \bar{f}(x) - f(x) ||^2] \\
&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| (f_{\mathcal{D}}(x) - \bar{f}(x)) + (\bar{f}(x) - f(x)) ||^2] \\
&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - \bar{f}(x)||^2 + || \bar{f}(x) - f(x)||^2 + 2||f_{\mathcal{D}}(x) - \bar{f}(x))^T(\bar{f}(x) - f(x)||]\\
 
&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - \bar{f}(x)||^2] + E_{\mathcal{D} \sim p(\mathcal{D})}[|| \bar{f}(x) - f(x)||^2] + E_{\mathcal{D} \sim p(\mathcal{D})}[2||f_{\mathcal{D}}(x) - \bar{f}(x))^T(\bar{f}(x) - f(x)||] \\
&=
\end{align}$$
- Steps
	1. Insert $- \bar{f}(x) + \bar{f}(x)$
	2. Regroup terms to form two groups $a = (f_{\mathcal{D}(x)} - \bar{f}(x)), b = (\bar{f}(x) - f(x))$
	3. Simplify the binomials ->  $||a + b||^2 = ||a||^2 + ||b||^2 + ||2ab||$
	4. 