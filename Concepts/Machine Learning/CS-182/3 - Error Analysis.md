# 3 Error Analysis
- Will we get the right answer?
## Empirical Risk and True Risk
- Zero-one loss -> 1 if wrong, 0 if right
- Risk -> probability of getting it wrong
- $$\huge \text{Risk} = E_{x \sim p(x),y \sim p(y | x)} [\mathcal{L}(x, y, \theta)]$$
- During training, we can't sample $x \sim p(x)$, we just have $\mathcal{D}$
- Instead we can calculate empirical risk
	- Empirical -> based on observation
- $$\huge \text{Empirical risk} = \frac{1}{2} \sum_{i = 1}^n \mathcal{L}(x_{i}, y_{i}, \theta) \approx E_{x \sim p(x), y \sim p(y | x)}[\mathcal{L}(x, y, \theta)]$$
- Is this a good approximation?
## Empirical Risk Minimization
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
## Let's Analyze Error!
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
	- Remember the model takes in $x$ and outputs a probability distribution of how likely each label $y$ is
		- To do this the model tries to learn the $p(y|x)$ distribution
			- See [[2 - Machine Learning Basics#^discriminative-learning|discriminative learning]]
	- This is from a normal distribution
- $\mathcal{N} =$ normal (Gaussian) distribution
	- Normal distributions are defined by a mean and the variance
	- Our model wants to optimize itself to match this distribution,
		- Therefore the mean and variance are defined as functions
		- This is a multivariate normal distribution
			- $f_{\theta}(x) =$ a vector of $y$ points
			- $\Sigma_{\theta}(x) =$ covariance matrix
				- Represents covariance between every possible pair of the $y$ points
- The normal distribution expands to
	- $$\large
	\begin{align}
	\log p_{\theta}(y | x) &= \mathcal{N}(f_{\theta}(x), \Sigma_{\theta}(x))  \\
	&= -\frac{1}{2} (f_{\theta}(x) - y)\Sigma_{\theta}(x)^{-1} (f_{\theta}(x) - y)) - \frac{1}{2} \log |\Sigma_{\theta}(x)| + \text{const}
	\end{align}
	$$
	- First term -> how close is the mean of theta to true label $y$
	- Second term -> tries to minimize the variance
	- Third term -> constant, that does not depend on $x$ and $y$
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
- Overfitting -> empirical risk is low, true risk is high
- How does the error change for different training sets?
	- ![[Pasted image 20250207130554.png]]
	- Overfitting
		- Training data is fitted well
		- True function is fitted poorly
		- **Learned function looks different each time**
		- Empirical risk is low, true risk is high
	- Underfitting
		- Training data is fitted poorly
		- True function is fitted poorly
		- **Learned function looks similar, even if we pool together all datasets**
		- Empirical risk is low high, true risk is high
- What is expected error, given a distribution **over datasets**?
	- ![[Pasted image 20250207131045.png]]
		- Dataset is produced according to some distribution $p(x,y) = p(x) p(y|x)$
	- $$\huge E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\theta}(x) - y ||^2]$$
		- Expected error w.r.t (with respect to) the data distribution
		- If we **repeatedly sample datasets** of the same size, what is our expected error across all samples?
			- Note that for every dataset $\mathcal{D}$, we get a different set of parameters $\theta$
			- Let's denote $f_{\theta}(x) = f_{\mathcal{D}}(x)$
			- Let's denote $y = f(x)$
				- $f(x)$ is the true function
				- $f_{\mathcal{D}}(x)$ is the learned function on the sample $\mathcal{D}$
- $$\huge \begin{align} &E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\theta}(x) - y ||^2] \\&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - f(x) ||^2] \\ &= \sum_{\mathcal{D}} p(\mathcal{D}) [|| f_{\mathcal{D}}(x) - f(x) ||^2] \end{align}$$
	- Expected error given a distribution over all datasets
		- Sum over all possible data sets' error scaled by the probability of getting each data set
	- Why do we care about this quantity
		- Want to understand how well our **algorithm** does independently of the **particular (random) choice of dataset**
			- How well does our algorithm perform in general?
		- This is important if we want to improve our algorithm
## Bias-Variance Tradeoff
$$\huge \bar{f}(x) = E_{\mathcal{D} \sim p(\mathcal{D})} [f_{D}(x)]$$
- $\bar{f}(x) =$ expected value of $f_{\mathcal{D}}(x)$ $= E_{\mathcal{D} \sim p(\mathcal{D}) }[f_{\mathcal{D}}(x)]$
	- Average of the predictions over all possible functions you'd get over all data sets (weighted by probability of each data set)
- $$\Huge E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - f(x) ||^2]$$
	- Starting from what we calculated in the previous text,
- $$\begin{align}
		&E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - f(x) ||^2] \\
		&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - \bar{f}(x) + \bar{f}(x) - f(x) ||^2] \\
		&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| (f_{\mathcal{D}}(x) - \bar{f}(x)) + (\bar{f}(x) - f(x)) ||^2] \\
		&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - \bar{f}(x)||^2 + || \bar{f}(x) - f(x)||^2 + 2||f_{\mathcal{D}}(x) - \bar{f}(x))^T(\bar{f}(x) - f(x)||]\\
		 
		&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - \bar{f}(x)||^2] + E_{\mathcal{D} \sim p(\mathcal{D})}[|| \bar{f}(x) - f(x)||^2] + E_{\mathcal{D} \sim p(\mathcal{D})}[2||f_{\mathcal{D}}(x) - \bar{f}(x))^T(\bar{f}(x) - f(x)||] \\
		&= E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - \bar{f}(x)||^2] + E_{\mathcal{D} \sim p(\mathcal{D})}[|| \bar{f}(x) - f(x)||^2] + 0 \\
		\end{align}$$
- Steps
	1. Insert $- \bar{f}(x) + \bar{f}(x)$
	2. Regroup terms to form two groups $a = (f_{\mathcal{D}(x)} - \bar{f}(x)), b = (\bar{f}(x) - f(x))$
	3. Simplify the binomials ->  $||a + b||^2 = ||a||^2 + ||b||^2 + ||2ab||$
	4. Third term cancels out
		1. $$\begin{align}
&E_{\mathcal{D} \sim p(\mathcal{D})}[2||f_{\mathcal{D}}(x) - \bar{f}(x))^T(\bar{f}(x) - f(x)||] \\
&= 2 E_{\mathcal{D} \sim p(\mathcal{D})}[f_{\mathcal{D}}(x) - \bar{f}(x)]^T \cdot (\bar{f}(x) - f(x)) \\
&= 2(E_{\mathcal{D} \sim p(\mathcal{D}) }[f_{\mathcal{D}}(x)] - \bar{f}(x))^T \cdot  (\bar{f}(x) - f(x)) \\
&= 2(E_{\mathcal{D} \sim p(\mathcal{D}) }[f_{\mathcal{D}}(x)] - E_{\mathcal{D} \sim p(\mathcal{D}) }[f_{\mathcal{D}}(x)])^T \cdot  (\bar{f}(x) - f(x)) \\
&= 2( \mathbf{0} )^T \cdot (\bar{f}(x) - f(x)) \\
&= \mathbf{0}
\end{align}$$
			- **NOTE:** The expected value we're calculating here is a **vector**
- $$
\huge \begin{align}
&= \underbrace{ E_{\mathcal{D} \sim p(\mathcal{D})} [|| f_{\mathcal{D}}(x) - \bar{f}(x)||^2] }_{ \text{Variance} } + \underbrace{ E_{\mathcal{D} \sim p(\mathcal{D})}[|| \bar{f}(x) - f(x)||^2] }_{ \text{Bias}^2 } \\
&= \text{Variance} + \text{Bias}
\end{align}
$$
- $\text{Bias}^2$
	- $\text{Bias} = E_{\mathcal{D} \sim p(\mathcal{D})}[|| \bar{f}(x) - f(x)||^2]$
		- How far off is the average prediction $\bar{f}(x)$ from the true function $f(x)$?
	- This error doesn't go away no matter how much data we have!
	- ![[Pasted image 20250403194633.png]]
- Variance
	- Regardless of what the true function is, how much does our prediction change with dataset?
		- **NOTE:** Overfitted models will fit to a specific dataset, therefore $f_{\mathcal{D}}(x)$ changes with every 
	- ![[Pasted image 20250403194753.png]]
- If **variance** is too high -> we have too little data/too complex a function class/etc.
	- This is **overfitting**
- If **bias** is too high -> we have insufficiently complex function class
	- This is **underfitting**
- How do we regulate the bias-variance tradeoff?
# Regularization
## How to regulate bias/variance?
- Get more data
	- Addresses variance
		- ![[Pasted image 20250403195951.png]]
	- Has no effect on bias
		- ![[Pasted image 20250403200006.png]]
- Change your model class -> change program
	- **Ex.** Changing 12th degree polynomial to linear functions
- Can we "smoothly" restrict the model class?
## Regularization
- **DEFINE:** Regularization
	- Something we add to loss function to reduce variance
- Bayesian interpretation
	- Could be regarded as a prior on parameters
- High level intuition
	- When we have high variance, it's because the data doesn't give enough information to identify parameters
		- **Many** different 12th degree polynomials that fit the function very well (according to empirical risk)
			- They're all equally good
	- If there's not enough data, can we give more information through loss function?
	- If we provide enough information to disambiguate between (almost) equally good models, we can pick the best one
		- ![[Pasted image 20250403200318.png]]
		- **Ex.** Empirical risk is the same for all of these models
		- However the rightmost model "looks" better because it's the smoothest
		- What makes third one better?
## The Bayesian Perspective
- Given dataset $\mathcal{D}$, what is most likely $\theta$
	- $\mathcal{D} = \{ (x_{1}, y_{1}), \dots \}$
	- $$\Huge \begin{align}
p(\theta \mid \mathcal{D}) &= \frac{p(\theta, \mathcal{D})}{p(\mathcal{D})} \propto p(\theta \mid \mathcal{D}) \\
p(\theta, \mathcal{D}) &= p(\mathcal{\mathcal{D} \mid \theta})p(\theta)
\end{align}$$
	- We can break up $p(\theta, \mathcal{D})$ using 
- We've seen $p(\mathcal{D} \mid \theta)$ before
	- See [[2 - Machine Learning Basics#How is the Dataset "Generated"?|the second lecture where we identified how D is generated]]
	- This is the maximum likelihood object
	- Note, $p_{\theta}(y_{i} \mid x_{i}) = p(y_{i} \mid x_{i}, \theta)$
- $p(\theta)$
	- How likely $\theta$ is before we've seen $\mathcal{D}$
	- This is called the prior
- Can we pick a prior that makes the smoother function more likely?
	- ![[Pasted image 20250403201151.png]]
- New loss function
	- $$\huge \mathcal{L}(\theta) = - \left( \sum_{i = 1}^N \log p(y_{i}\mid x_{i}, \theta)\right) - \underbrace{ \log p(\theta) }_{ \text{our prior} }$$
	- We choose the prior we tack onto the loss function
## Example: regularized linear regression
- Can we pick a prior to make the smoother function more likely?
- Example linear regression with polynomial features
	- $f_{\theta}(x) = \theta_{0} + \theta_{1}x + \theta_{2}x^2 + \dots$
	- ![[Pasted image 20250403201633.png]]
	- Spiky graphs typically require large coefficients
	- If we only allow small coefficients, the best fit is smoother
- What kind of distributions assigns higher probabilities to small numbers?
	- Simple idea: $p(\theta) = \mathcal{N}(0, \sigma^2)$
	- ![[Pasted image 20250403201607.png]]
	- $$\huge \begin{align}
\log p(\theta) &= \sum_{i = 1}^D -\frac{1}{2} \frac{\theta_{i}^2}{\sigma^2} \underbrace{ - \log \sigma - \frac{1}{2} \log 2 \pi }_{ \text{doesn't influence } \theta } \\
&= -\lambda \lVert \theta \rVert^2 + \text{const} \\  \\
\lambda &= \frac{1}{2 \sigma^2} \hspace{2em} \text{hyperparameter}
\end{align}$$
	- Let's assume $\lambda$ is a variable that we choose for ourselves
		- We call this a "hyperparameter"
- Final formula
	- $$\huge \mathcal{L}(\theta) = - \left( \sum_{i = 1}^N \log p(y_{i}\mid x_{i}, \theta)\right) - \lambda \lVert \theta \rVert^2 $$
	- If $\sum_{\theta}(x) = \mathbf{I}$
		- This means there is no correlation between the inputs $x$
		- The covariance is zero between all inputs $x$
## Example: regularized logistic regression
- Logistic regression -> classification
- ![[Pasted image 20250403202725.png]]
- What we want is a single line, with smooth transitions between $P(\text{red})$ and $P(\text{blue})$
- However what if we got a more jumbled, and sharp decision boundary?
	- If points are moved around, the decision boundary may change drastically
- **Technically** every point is classified correct, but it's overfitting
- Recall what we learned about the softmax function
	- ![[Pasted image 20250403203025.png]]
	- Larger $\theta$ -> sharper probabilities
	- Solution, prefer smaller $\theta$
		- Simple idea, use normal distribution (again): $p(\theta) = \mathcal{N}(0, \sigma^2)$
- Loss
	- $$\huge \mathcal{L}(\theta) = -\left( \sum_{i = 1}^N \log p(y_{i} \mid x_{i}, \theta) \right) + \lambda \lVert \theta \rVert^2 $$
	- This is the same prior as the linear regression
		- This is sometimes called **weight decay**
	- Other samples of regularizers
		- L1 regularization -> $\lambda \sum \theta^2$
		- L2 regularization -> Absolute value
			- $$\huge \lambda \sum_{i = 1}^D \lvert \theta_{i} \rvert$$
			- ![[Pasted image 20250403203227.png]]
		- Drop out
			- Special type of regularizer for neural networks
		- Gradient penalty
			- Special type of regularizer for GANs (generative adversarial networks)
		- Lots of other choices...

> [!note]
> - Absolute value encourages every dimension to be zero
> - Consider geometric representation of the two regularizers (L1 and L2) on two weights $\theta_{1}$ and $\theta_{2}$
> 	-  ![[Pasted image 20250403203255.png]]
> - Red ellipses are contours of a log likelihood
> 	- Concentric circles indicate circles of increasing log likelihood
> - Blue shapes
>	- Represent contours of regularizer
>	- All regularizers try to drive weight towards zero (darkest region)
> - Find point in space where regularizer and loglikelihood balance out -> depicted as an intersection between two
> - L2 regularization favors zeroing out dimensions (a specific weight $\theta_{i}$)

## Other perspectives
- Regularization -> something we add to loss function to reduce variance
- Bayesian perspective -> regularizer is prior knowledge about parameters
- Numerical perspective -> regularizer makes undetermined problems well-determined
	- L2 regularization can make a singular matrix (non-invertible) into a non-singular matrix (invertible)
		- Therefore by making it invertible, we have one possible solution?
- Optimization perspective -> regularizer makes the loss landscape easier to search
	- Paradoxically, regularizers can sometimes reduce underfitting if it was due to poor optimization
	- Especially common in GANs
- In general, any "heuristic" term added to loss that doesn't depend on data is generally called a regularizer
	- Regularizers introduce hyperparameters that we have to select in order for them to work well
	- Hyperparameters cannot depend on training loss, because training loss is already low
	- **Ex.** $\lambda =$ hyperparameter in L2 norm
		- $\lambda \lVert \theta \rVert^2$
# Training sets and test sets
- Some questions
	- How do we know if are overfitting or underfitting
	- How do we select which algorithm to use?
	- How do we select hyperparameters?
	- One idea -> choose whatever makes the loss low
		- $\mathcal{L}(\theta) = 0$
		- Drawback -> only tells us about bias, nothing about variance
		- Can't diagnose overfitting
## Machine learning workflow
- Take dataset and divide it into a **training set** and **validation set**
	- Training set
		- Used for training
		- $\mathcal{L}(\theta, \mathcal{D}_{\text{train}})$
			- Lets call this new notation the "training loss"
	- Validation set
		- Reserve this for 
			- Selecting hyperparameters
			- Adding/removing features
			- Tweaking model class
		- $\mathcal{L}(\theta, \mathcal{D}_{\text{val}})$
			- Loss on the "validation set"
		- Helps us diagnose overfitting, because the data hasn't been used for training
- Workflow
	1. Train $\theta$ with $\mathcal{L}(\theta, \mathcal{D}_{\text{train}})$
		1. If $\mathcal{L}(\theta, \mathcal{D}_{\text{train}})$ not low enough, you are underfitting
			1. Decrease regularization
			2. Improve your optimizer (adding more features, bigger models)
	2. Look at $\mathcal{L}(\theta, \mathcal{D}_{\text{val}})$
		1. If $\mathcal{L}(\theta, \mathcal{D}_{\text{val}}) > > \mathcal{L}(\theta, \mathcal{D}_{\text{train}})$
			1. If loss of validation set is drastically bigger than loss from training dataset, then you are overfitting
			2. Increase regularization
			3. Then go back to step one and repeat process
- Hyperparameters
	- Training set -> Used to select 
		- $\theta$ via optimization
		- Optimizer hyperparameters (**Ex.** learning rate $\alpha$)
			- **NOTE:** Does not include 
	- Validation set -> used to select
		- Model class (logistic regression, neural net)
		- Used to inform parameters that influence overfitting
			- Regularizer hyperparameters
			- Which features to use
## Learning curves
- Learning curves
	- Horizontal axis -> Number of gradient descent steps (optimization progress, number of iterations)
	- Vertical axis -> loss
	- Often look at training and validation loss together, plotted side by side
- Examples
	- ![[Pasted image 20250403210125.png]]
		- Analysis
			- Initially, both training and validation loss are high
				- Usually training loss is lower
			- Eventually, validation loss increases as we overfit, even though training loss continues to decrease
			- Indicates overfitting
	- ![[Pasted image 20250403210154.png]]
		- Analysis
			- Validation and training loss stay pretty similar, but they don't go down anymore
			- The gap is the bias
			- Indicates underfitting
- Question -> Can we stop when regularization is low to avoid overfitting?
	- How do we know when to stop?
		- Look for when the validation loss starts to increase, then go back a few steps
		- Known as **early stops** in deep learning
	- Drawback
		- We might not be able to push down the training and validation loss as low as we could have, if we had stuck with regularization
## Final exam (for the model)
- We followed recipe, now what?
- How good is our final classifier?
	- $\mathcal{L}(\theta, \mathcal{D}_{\text{val}})$
- That's no good, we already used the validation set to pick hyperparameters
	- Therefore we cannot rely on optimization loss to be unbiased
	- Not statistically independent from model anymore
- What if we did same thing again?
## Machine learning workflow (complete)
- Partition dataset from training set, validation set, and test set
	- We never look at/use the test set at all for training/tuning our model
- Partition the data set into
	- Training set -> Used to select
		- $\theta$ via optimization
		- Optimizer hyperparameters
	- Validation set -> used to select
		- Model class
		- Regularizer hyperparameters
		- Which features to use
	- Test set -> Only to report final performance
## Summary and takeaways
- Where to errors come from?
	- Variance -> too much capacity
		- Not enough information in data to find right parameters
	- Bias -> too little capacity
		- Not enough representational power to represent the true function 
	- $\text{Error} = \text{Variance} + \text{Bias}^2$
	- Overfitting -> too much variance
	- Underfitting -> too much bias
- How can we trade off bias and variance?
	- Select model class carefully
	- Select your features carefully
	- Regularization -> stuff we add to loss to reduce variance
- How do we select hyperparameters?
	- Training/validation split
	- Training set is for optimization (learning)
	- Validation set for hyperparameters
	- Test set is for reporting final results and nothing else!