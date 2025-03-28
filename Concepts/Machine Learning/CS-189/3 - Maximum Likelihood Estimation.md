# 3 Maximum likelihood estimation
- We'll cover one of the main principles of "learning" (fitting, estimating) good parameters for machine learning (and more generally, statistical models)
- Maximum likelihood estimation (MLE)
- We will go over core concepts and provide examples
- Next lecture, discuss how principle applies to ML and connections to other approaches
# Recall: What is machine learning?
- Three core concepts -> model, optimization, data
- Model has parameters are optimized (learned)
- Optimization algorithm finds parameters that are good fit for training data
	- How do we define a "good fit"?
- We need a **loss function**
	- Measures how good parameters are
	- Takes in parameters, and outputs single value, the loss
	- Higher loss -> worse parameters are
	- Lower loss -> better parameters are
- Learning objective is to find parameters to minimize the loss
# Loss functions and objectives
![[Pasted image 20250328114308.png]]
- Data: $\{ (x_{i}, y_{i}) \}^n_{i = 1}$
	- Data has $n$ points
	- Each point is $(x_{i}, y_{i})$
- Model: $f_{\theta}(x) = y$
	- Linear model $f\theta(x) = w^T x + b$
	- $\theta = [w, b]$
- Loss function:
	- $l(y, \hat{y}) = (y - \hat{y})^2$
	- $\hat{y}$ = a prediction
- Objective -> find parameters that minimize the average loss
	- $$\huge \begin{align}
\theta^* &= \underset{ \theta }{ \arg \min } \frac{1}{N} \sum_{i = 1}^N l(y_{i}, f_{\theta}(x_{i})) \\ \\
&= \underset{ \theta }{ \arg \min } \frac{1}{N} \sum_{i = 1}^N (y_{i} - f_{\theta}(x_{i}))^2
\end{align}$$
	- $\theta^*$ = set of ideal parameters
# The maximum likelihood principle
- Require model to define a probability distribution over the data
	- Distribution is controlled by the model parameters
	- We will focus on **probabilistic models**
		- Models that define probability distributions over the data
- Assume some setting of the parameters generated the observed data
	- Assume the observed data were sampled from a distribution corresponding to the specified model with some unknown parameters
- Maximum likelihood principle
	- We can recover these parameters through optimization, by maximizing the likelihood of the observed data
# Probabilistic models
- What does a probabilistic model look like?
- From linear to linear-gaussian
	- Linear -> $f_{\theta}(x) = wx + b$
	- Linear-gaussian -> $f_{\theta}(x) = \mathcal{N}(wx + b, \sigma^2)$
		- Gaussian distribution -> $\mathcal{N}(\mu, \sigma^2)$
			- $\mu$ = mean of distribution
			- $\sigma^2$ = variance of distribution
		- The mean of this distribution $wx +b$
			- It's not guaranteed that the output will always be $wx + b$, but the average output from many samples will be $wx + b$
		- This is called a linear-gaussian
		- ![[Pasted image 20250328115909.png]]
- We now talk in terms of output probabilities
	- $P_{\theta}(y \mid x) = \mathcal{N}(y; wx + b, \sigma^2)$
		- **NOTE:** $\mathcal{N}$ here uses three terms, and is a probability density function
			- This is the probability of getting $y$ given the gaussian distribution with the mean of $wx + b$ and a variance of $\sigma^2$
	- We're interested in the probability that we get a certain output given an input
# MLE for machine learning?
- MLE is general tool for statistical inference -> learning what parameters explain data
	- Most of machine learning is statistical inference
- Machine learning cares more on models that go from inputs to outputs
	- Statistics likes to focus on models such as population level model of the data
- Don't think as hard as model is **misspecified**
	- Previously, we "assumed" there is a linear relationship between input and output
	- However this isn't always the case -> actual relationship in data might not be linear
 - We will start with non machine learning overview of MLE
# MLE: Basic setup
- Given data $\mathcal{D} = \{ \text{x}_{1}, \dots, \text{x}_{N} \}$
	- $\text{x}$ can be $n$ dimension vector
- Assume set (family) of distributions on $\text{x}$
	- $\{ P_{\theta} : \theta \in \Theta \}$
	- For example, the set of distributions could be all possible Gaussian distributions
	- $\theta$ = parameters of the distributions
		- **Ex.** Gaussian parameters = mean + variance
		- $\Theta$ = set of all possible parameters
	- $P_{\theta}$ would be a specific a distribution with a specific set of parameters
		- **Ex.** Gaussian distribution with mean of 5, and variance of 6
- Assume $\mathcal{D}$ was sampled from a member of this family of sets
	- $\text{x}_{1}, \dots, \text{x}_{N} \overset{ \text{i.i.d.} }{ \sim } P_{\hat{\theta}}$ for some $\hat{\theta} \in \Theta$
- Goal -> recover $\hat{\theta}$
- Objective/definition
	- $$\huge \begin{align}
		\theta_{MLE} &= \underset{ \theta \in \Theta }{ \arg \max } \; P_{\theta}(\mathcal{D}) \\
		&= \underset{ \theta \in \Theta }{ \arg \max } \prod_{i=1}^N P_{\theta}(\text{x}_{i}) \\
 	&= \underset{ \theta \in \Theta }{ \arg \max } \sum_{i=1}^N \log P_{\theta}(\text{x}_{i})
		\end{align} $$
	- Choose $\theta$ that maximizes probability of seeing our observed data (seeing all the data points together)
# Example: MLE for a univariate Gaussian
- Suppose you were given set of height measurements
	- You believe underlying distribution of heights s Gaussian
	- Pretty reasonable modelling assumption
	- ![[Pasted image 20250328121802.png]]
- How would you estimate mean + variance of Gaussian from data?
- First example of MLE in action
- Not really machine learning
	- Aren't predicting anything
	- Once we have the model, we can't use it for prediction, it just tells us the model of a specific population
- Assume each data point is i.i.d as $X_{i} \sim \mathcal{N}(\hat{\mu}, \hat{\sigma}^2)$
	- $\theta = [\mu, \sigma^2]$
- We are given $\mathcal{D} = \{ x_{1}, \dots, x_{N} \}$
- Goal is to find 
	- $$\huge \theta_{MLE} = [\mu_{MLE}, \sigma_{MLE}^2] = \underset{ \mu, \sigma^2 }{ \arg \max } \prod_{i}^N \mathcal{N}(x_{i}; \mu, \sigma^2)$$
- How? Take derivative and set to zero (and check second derivatives to avoid saddles, etc.)
	- Doing this for product of terms is tricky and messy
- Idea -> take the log, always!
	- Log -> monotonically increasing function
- $\log$ is a monotonically increasing function
	- $$\large \begin{align}
\underset{ \mu, \sigma^2 }{ \arg \max } \prod_{i}^N \mathcal{N}(x_{i}; \mu, \sigma^2) &= \underset{ \mu, \sigma^2 }{ \arg \max } \log\prod_{i}^N \mathcal{N}(x_{i}; \mu, \sigma^2) \\
&= \underset{ \mu, \sigma^2 }{ \arg \max } \sum_{i}^N  \log \left[  -\frac{1}{2} \frac{(x_{i} - \mu)^2}{\sigma^2} - \frac{1}{2} \log \sigma^2 + C \right] \\
&= \underset{ \mu, \sigma^2 }{ \arg \max } \sum_{i}^N  \log \mathcal{N}(x_{i}; \mu, \sigma^2)  \\
&= \underset{ \mu, \sigma^2 }{ \arg \max } \frac{1}{2\sigma^2} \sum_{i = 1}^N (x_{i} - \mu)^2 - \frac{N}{2} \log \sigma^2
\end{align}$$
	- The log of a product of terms can be rewritten into the sum of the log of each term
	- We can also expand $\mathcal{N}$ as well
		- **NOTE:** $C$ is a constant term the results from expanding $\mathcal{N}$
			- Since it's a constant, we can remove it in our from the maximization problem, because we only care about the relative differences between possible parameters
- We wish to find
	- $$\huge \underset{ \mu, \sigma^2 }{ \arg \max } \frac{1}{2\sigma^2} \sum_{i = 1}^N (x_{i} - \mu)^2 - \frac{N}{2} \log \sigma^2$$
	- We will proceed by taking (partial) derivatives and setting equal to zero
- $$\huge \begin{align}
\underset{ \mu, \sigma^2 }{ \arg \max } -\frac{1}{2}\left[ \left( \sum_{i} \frac{(x_{i} - \mu)^2}{\sigma^2} + \log \sigma^2 \right) + C \right] \\
\frac{ \partial }{ \partial \mu } = \sum_{i} \frac{x_{i} - \mu_{MLE}}{\sigma^2} = 0 \\
\mu_{MLE} = \frac{\sum_{i} x_{i}}{N} \\
\\ \\
\frac{ \partial }{ \partial \sigma^2 } = \frac{1}{2(\sigma^2_{MLE})^2} \sum_{i}(x_{i} - \mu)^2 - \frac{N}{2 \sigma_{MLE}^2} = 0 \\
\sigma^2_{MLE} = \frac{\sum_{i} (x_{i} - \mu)^2}{N}
\end{align}$$