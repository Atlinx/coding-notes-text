# 5 Linear Regression
- Last lecture -> related least squares regression to MLE
- Today -> continue discussion of linear regression, recap solution from last time and examining it in greater detail
- Different assumptions of data generation process leads to different MLE formulations of linear regression
- Take geometric view of linear regression
# From MLE to least squares linear regression
- MLE objective
	- $$\huge \underset{ \text{w} }{ \arg \max } - \frac{1}{2\sigma^2} \sum_{i = 1}^N  (\text{w}^T \text{x}_{i} - y_{i})^2 = \underset{ \text{w} }{ \arg \min } \sum_{i = 1}^N (\text{w}^T \text{x}_{i} - y_{i})^2$$
	- We can remove the negative by turning the maximization problem ($\arg \max$) into an minimization problem ($\arg \min$)
	- We can remove the $\dfrac{1}{2\sigma^2}$ because it doesn't change with respect to $\text{w}$, therefore can be treated as a constant
	- Here the bias term has been dropped for convenience
		- **NOTE:** We can include the bias term by putting a $1$ at the end of the $\text{x}_{i}$ vector, and then add the bias to the end of the $\text{w}$ vector
		- Lets do some linear algebra on this MLE objective
- This MLE objective looks like $l_{2}$ norm squared
	- $(\lVert \text{v} \rVert_{2})^2 = \left( \sqrt{ \sum_{i} v_{i}^2 } \right)^2 = \sum_{i} v_{i}^2 = \text{v}^T\text{v}$
		- Specifically, this look likes the $\sum_{i = 1}^N (\text{w}^T \text{x}_{i} - y_{i})^2$, where  $v_{i} = (\text{w}^T \text{x}_{i} - y_{i})$
	- $\text{w}^T\text{x}_{i} - y_{i}$ is the $i^{th}$ element of what vector?
	- $$\text{y} = \begin{bmatrix}
		y_{1} \\
		\vdots  \\
		y_{N}
		\end{bmatrix}
		\hspace{3em}X = \begin{bmatrix}
		— &\text{x}_{1}^T &— \\
		&\vdots \\
		— &\text{x}_{N}^T &—
		\end{bmatrix}
		$$
	- We can collect inputs and collect them into a matrix $X$
- $X$ is design matrix
	- Matrix of data point vectors $\text{x}_{i}$
	- $(X\text{w} - y) = \text{w}^T\text{x}_{i} - y_{i}$
		- See how on the right, we are taking the inner product between $i^{th}$ input and $\text{w}$
	- Therefore, the objective function is equivalent to
		- $$\huge \underset{ \text{w} }{ \arg \min } \sum_{i = 1}^N (\text{w}^T \text{x}_{i} - y_{i})^2 = \underset{ \text{w} }{ \arg \min } \; (\lVert X \text{w} - \text{y} \rVert_{2})^2$$
# Solving least squares linear regression
- Objective 
	- $$\huge \begin{align}
		\underset{ \text{w} }{ \arg \min } \; (\lVert X \text{w} - \text{y} \rVert_{2})^2 &= \underset{ \text{w} }{ \arg \min } \; (X \text{w} - \text{y} )^T (X \text{w} - \text{y} ) \\
		&= \underset{ \text{w} }{ \arg \min } \; \text{w}^T X^T X \text{w} - 2\text{y}^TX \text{w} + \text{y}^T \text{y}
		\end{align}$$
	- From before we know that ($\lVert v \rVert_{2})^2 = v^Tv$, therefore we can rewrite $(\lVert X\text{w} - \text{y} \rVert_{2})^2 = (X \text{w} - \text{y})^T(X \text{w} - \text{y})$
	- Afterwards we can expand equation using FOIL
- Lets take the gradient of this (quadratic form) and set equal to zero
	- $$\huge \begin{align}
		\nabla_{\text{w}} = 2 X^T X \text{w} - 2X^T \text{y}
		\end{align}$$
	- **NOTE:** Taking a derivative with vector math is similar to taking derivatives with scalars
		- Except we must apply the transpose operator whenever we perform the power rule
		- See the [[2 - Math Review#^matrix-cookbook|matrix cookbook]] for more information on vector calculus
	- Set the gradient to $0$, and solve for $\text{w}_{MLE}$
		- $$\huge \begin{align}
			X^TX\text{w}_{MLE} = X^T \text{y}  \\
			\text{w}_{MLE} = (\text{X}^T \text{X})^T \text{X}^T \text{y}
			\end{align}$$
			
		- This is the classical solution to the least squares linear regression problem
		- **NOTE:** This requires $X$ to be orthogonal
			- We're assuming $X^TX = I$, whereas normally only $X^{-1}X = I$
			- This means we're assuming $X^T = X^{-1}$, which is only true for orthogonal matrices
- Hessian (second derivative) check
	- $$\huge \begin{align}
		\nabla_{\text{w}}^2 = 2 X^TX
		\end{align}$$
	- We claim this is PSD (positive semidefinite), in order to guarantee that the 0 point we found previous is indeed the global minimum, rather than a saddle, or another point.
	- How do we check?
		- Recall that PSD matrices must satisfy $\text{x}^T A \text{x} \geq 0$ for all $\text{x}$
		- $$\begin{align}
			&\text{v}^T (2X^TX)\text{v}  \tag{1}\\
			&= 2(X\text{v})^T X \text{v}  \tag{2}\\
			&= 2 \lVert X \text{v} \rVert_{2}^2 \geq 0 \tag{3}\end{align}$$
			- Step $(1)$, we add wrap $\text{v}^T$ and $\text{v}$ around the previous equation
				- We're allowed to do this because this is what the [[2 - Math Review#^psd-matrix|definition of a PSD matrix]] calls for
			- Step $(2)$, we regroup the terms
			- Step $(3)$, we know that $X\text{v}$ is a vector, let's call this $\text{u}$
				- Then we know $\text{u}^T\text{u} = \sum_{i} u_{i}^2$
				- This also happens to be the definition the square of the $l_{2}$ norm, see [[2 - Math Review#^l2-norm|Math Review]]
				- Since $l_{2}$ norm is squared, this means it is always positive
				- Therefore, the matrix $X^TX$ is a PSD matrix
			- This is definition of PSD matrix
			- Therefore $X^TX$ is positive semidefinite
# Examining the least squares solution
- Solution we found needs $X^TX$ to be invertible -- when is this not the case?
	- Not invertible when the columns of $X$ are not linearly independent
		- Rows of $X$ are datapoints
		- Columns of $X$ are linearly independent
	- AKA not invertible when certain features of input are (linearly) determined by a set of other features
	- If it is invertible, there are an infinite number of solutions!
- Intuitively, we make think to simply remove features that area already determined by other feature
	- Form of **feature selection**
- May consider other constraints on the solution, such as minimizing the norm of the solution
	- If we add another parameter to minimize, then we can potentially force the solutions to be finite again
	- Form of **regularization**
# An aside: feature selection
- If we believe that we don't need entire set of features, can we "prune" them?
- Many ideas -> simplest is to either select or prune features greedily
	- **Forward selection**
		- Initialize empty set of features
		- For each feature, add it to set and train the model
		- Pick the best feature to add
		- Repeat (until you see you aren't getting a good performance boost)
	- **Backward elimination**
		- Initialize full set of features
		- For each feature, remove it, and train the model
		- Pick best feature to remove (model suffers the least when the feature is removed)
			- For linearly dependent features, the performance should be hurt very little when removed
		- Repeat
- Most machine learning relies on regularization, than feature selection
# Other MLE formulations for linear regression
- Specific assumption of i.i.d. Gaussian noise on each output, conditioned on the input, led to MLE being equivalent to least squares linear regression
- What about other assumptions?
	- What if the data point is not from a Gaussian distribution?
- What if the i.i.d. noise instead follows a Laplace distribution?
	- Will see this leads to **least absolute deviation** linear regression
- What if the noise is not i.i.d.?
	- Will examine case where difference variances for each data point and how this leads to weighted linear regression
	- Still will assume independence, but no longer from same distribution
# From MLE to least absolute deviation
- Assume output given input is generated i.i.d.
	- $Y \mid X \sim \text{Laplace}(\hat{\text{w}}^TX, \sigma)$
		- Where $\text{Laplace}(y; \mu, \sigma) = \frac{1}{2\sigma} \exp \left\{  - |\frac{y - \mu |}{\sigma}  \right\}$
			- Similar to gaussian, expect it has absolute value instead of square inside of the $\exp$
			- **NOTE:** The follow notation is the same $\exp x = e^x$
- Objective is
	- $$\huge \begin{align}
&\underset{ \text{w} }{ \arg \max } \sum_{i = 1}^N \log \text{Laplace}(y_{i}; \text{w}^T\text{x}_{i}, \sigma) \\
&= \underset{ \text{w} }{ \arg \max } \sum_{i = 1}^N - \frac{1}{\sigma} |y_{i} - \text{w}^T\text{x}_{i}| + C_{\text{w.r.t w}}
\end{align}$$
	- This objective is known as "least absolute deviation" linear regression 
# Lead absolute deviations vs. least squares
- Sadly, least absolute deviations has no analytical closed form solution
	- Cannot directly solve for $\sigma$
		- Can differentiate the gradient, but setting gradient = 0 doesn't help us solve for $\text{w}$
	- We must use iterative optimization -> most common used methods for this problem are **simplex methods**
- Why might we choose this over least squares?
	- One reason is least absolute deviation is more robust to outliers
	- ![[Pasted image 20250328201218.png]]
	- Yellow line is least absolute deviation
	- Least squares is affected by the outlier
		- Squaring the outlier makes it larger, growing exponentially the farther away it is from the 
- Robustness is thanks to properties of heavy tailed distributions
	- ![[Pasted image 20250328201322.png]]
	- Black distribution above is a normal distribution
	- Red distribution (Cauchy) above has a **heavy-tail**
		- Heavy tail distributions peak higher
		- Keeps a reasonable amount of probabilities for points away from the mean
			- Tip does not hit the x-axis as quickly
		- Heavy tail assigns more probability to outliers than in a Gaussian distribution, which tapers off very quickly
			- This means the point isn't penalized as hard for being an outlier
# From MLE to weighted linear regression
- Assume that output given the input is generated independently (not i.i.d!) as
	- $Y_{i} | X_{i} \sim \mathcal{N}(\hat{\text{w}}^T X_{i}, \sigma_{i}^2)$
		- Assume $\hat{\text{w}}$ is the same across data points
		- Assume there are different noise levels across data points ($\sigma_{i}^2$)
- Objective is
	- $$\huge \begin{align}
&\underset{ \text{w} }{ \arg \max } \sum_{i = 1}^N \log \mathcal{N}(y_{i}; \text{w}^Tx_{i}, \sigma_{i}^2) \\
&= \underset{ \text{w} }{ \arg \max } \sum_{i = 1}^N -\frac{1}{2\sigma_{i}^2} (\text{w}^T \text{x}_{i} - y_{i})^2 + C \\
&= \underset{ \text{w} }{ \arg \max } \sum_{i = 1}^N -\frac{1}{2\sigma_{i}^2} (\text{w}^T \text{x}_{i} - y_{i})^2 + C \\
\end{align}$$
	- We cannot just take out the $\sigma$ now!
		- $(\text{w}^T\text{x}_{i} - y_{i})^2 = (X \text{w} - \text{y})^T \Sigma (X \text{w} - \text{y})$
	- $-\dfrac{1}{2\sigma^2_{i}}$ becomes the "weight" for each error term
- This is useful for more than just when we "trust" some data more than other data
	- We may "care more" about getting the right answer for some data
	- **Ex.** Predicting diseases given cholesterol levels
		- Maybe there are certain ranges of cholesterol levels to get accurate predictions for
# Geometric perspective
- Let $\hat{\text{y}} =$ predictions for the following slides
- What predictions $\hat{\text{y}} = X\text{w}$ are even possible for the model to make?
	- Predictions $\hat{\text{y}}$ are constrained to be in column space of $X$ (span of the column vectors of $X$)
- Targets $\text{y}$ are almost certainly not in column space of $X$ due to noise, outliers, etc.
- We wish to find predictions $\hat{\text{y}}$ in column space of $X$ that are as close as possible (in terms of $l_{2}$) to $\text{y}$
	- This is the projection of $\text{y}$ into the column space of $X$
- $\lVert \text{y} - \hat{\text{y}} \rVert^2_{2}$
	- Want to minimize this ($l_{2}$ norm)
	- $(\text{y} - \hat{\text{y}})X = 0$
- Remember, orthogonal complement of the column space is the (left) null space