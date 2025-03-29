# 2 Math Review
# Probability review
- Random variables
	- Takes on values based on outcome of random event
	- **Ex.** Outcome of coin flip, dice roll, random sample from population
- Random variables are denoted with capital letter
- Sample (realization) of random variable is typically denoted with a lower case letter
- Often talk about independent and identically distributed (i.i.d.) samples
	- Samples do not influence each other
		- **Ex.** Pulling 5 cards from a deck without replacement would not be independent, because the probability would decrease
	- Samples come from the same probability distribution
- Often rely on formulas, like Baye's rule, Jensen's inequality
## Random variables
- Let $X$ be R.V. (Random variable) denoting outcome of flipping a biased coin
	- $P(X = 1) = 0.75$
		- Probability of getting heads = 0.75
	- $P(X = 0) = 0.25$
		- Probability of getting tails = 0.25
	- $X \sim \text{Bernoulli}(0.75)$
		- X is distributed according to a Bernoulli distribution
- Flipping the coin $N$ times
	- $$\huge X_{1}, \dots X_{N} \sim \text{Bernoulli}(0.75)$$
- Expected value 
	- $$\huge \mathbb{E}[X] = \sum_{x \in \{0, 1\}} P(X = x) = 0.75$$
- Variance
	- $$\huge \text{Var}(X) = \mathbb{E}[x^2] - \mathbb{E}[x]^2 = 0.75 - 0.5625 = 0.1875$$
		- Variance is always non-negative
			- [Proved using Jensen's inequality](https://www.probabilitycourse.com/chapter6/6_2_5_jensen's_inequality.php)
## Information Theory
- Class will use basic concepts from information theory
	- How much information is present in random variables?
- Entropy of a random variable
	- Sometimes referred to as "expected suprise"
	- **Ex.** 
		- ![[Pasted image 20250328090725.png]]
		- Entropy is low if the coin always comes up as heads (0) or always comes up as tails (1), we always expect
		- If we have a perfectly balanced coin (0.5), then we don't know what to expect when we flip the coin
			- This equates to entropy of 1
		- Maximum entropy is when the random variable is uniform (all outcomes have the same probability)
- Entropy
	- Denoted as $H(X)$ or sometimes $H(P)$, where $P$ is the probability distribution of random variable $X$
	- $$\huge \begin{align}
		H(X) &= - \sum_{x} P(X = x) \log  P(X = x) \\
		&= \mathbb{E}[- \log P(X = x)]
		\end{align} $$
	- $- \log P(X = x)$ 
		- The less probable the value is, the higher the negative log probability is
		- The more surprised we'd be
	- For $X \sim \text{Bernoulli}(0.75)$
		- $H(X) = - (0.75 \log 0.75 + 0.25 \log 0.25) \approx 0.811 \text{ bits}$
		- What base of log?
			- Base 2 -> denote information as bits
			- Base 10 -> denotes information as nats
- Cross-entropy
	- $$\huge \begin{align}
H(P, Q) &= - \sum_{x} P(X = x) \log Q(X = x) \\ \\
&= \mathbb{E}_{P}[- \log Q(X = x)]
\end{align}$$
	- Subscript under expectation $\mathbb{E}$ means which distribution the random variable inside the expectation follows
	- Measurement of how similar distributions $P$ and $Q$ are
	- If $P$ and $Q$ are same distribution, then they have the same entropy value
		- This is minimum cross-entropy value
- Kullback-Leiber (KL) divergence (relative entropy)
	- $$\huge \begin{align}
		D_{KL}(P \parallel Q) &= \sum_{x} P(X = x) \log \frac{P(X = x)}{Q(X = x)} \\ \\
		&= H(P, Q) - H(P)
		\end{align}$$
	- KL divergence is always non-negative
	- 0 when P and Q are the same
	- Not a proper measure of "distance"
		- Not symmetric -> $D_{KL}(P \parallel Q) \neq D_{KL}(P \parallel Q)$
	- Grounds the divergence at 0
- Monte Carlo estimation
	- $$\huge \mathbb{E}_{P}[f(X)] = \frac{1}{N} \sum_{i = 1}^N f(x_{i})$$
	- Difficult to compute the expected value of a function
	- What if drew $N$ samples of $x_{1}, \dots x_{N} \underset{ i.i.d }{ \sim } P$
		- Samples are independent and identically distributed, drawn from distribution $P$
# Linear algebra review
##  Vectors, matrices, and tensors
- Vector -> "one dimensional" row or column of (usually) numbers
	- "one dimensional" here means it has a single row
	- **NOTE:** Dimension in this class tend to refer to the components of the vector
		- **Ex.** $[x, y] =$ 2 dimensional vectors
- Matrix -> "two dimensional" table
- Tensors -> Higher dimensional objects
	- **Ex.** A 3D matrix
- Are all denoted with bold and non-cursive letters, when possible
- Subscripts are used for a lot of different purposes
	- Tend to denote different entries in vectors, matrices, etc.
- $u = [u_{1}, \dots, u_{d}]$
	- $u$ is a $d$-dimensional row vector
- $\text{v} = \begin{bmatrix}v_{1} \\ \vdots \\ v_{k} \end{bmatrix}$
	- $\text{v}$ is a $k$-dimensional column vector
- $l_{p}$-norm
	- $$\huge \lVert \text{u} \rVert_{p} = (\sum_{i} |u_{i} |^p)^{\normalsize \dfrac{1}{p}}$$
	- LP norm of some vector $u$ is sum of all entries
	- $l_{1}$ norm
		- $=\sum_{i} |u_{i}|$
	- $l_{2}$ norm ^l2-norm
		- $= \sqrt{ \sum_{i} u_{i}^2 }$
		- AKA distance formula
- Matrices
	- $A = \begin{bmatrix}a_{11} &\dots &a_{1m} \\ \vdots &\ddots &\vdots \\ a_{n_{1}} &\dots &a_{nm}\end{bmatrix}$
	- Square matrix -> $n = m$
	- Symmetric matrix $a_{ij} = a_{ji}$
		- $A = A^T$
	- Positive semidefinite (PSD) matrix ^psd-matrix
		- A square and symmetric matrix $A$ for which
			- $\text{x}^T A \text{x} \geq 0$ for all $\text{x}$
			- If $A$ is $n \times n$, then $x$ is any $n$ dimensional column vector
			- Don't really distinguish between row vector and column vectors, depending
- Trace of a square matrix
	- $tr(A) = \sum_{i} a_{ii}$
		- Sum of diagonal entries of matrix
- Frobenius norm
	- $\lVert A \rVert_{F} = \sqrt{ \sum_{i} \sum_{j} a_{ij}^2 } = \sqrt{ tr(A^TA) }$
		- Square all entries of matrix, then sum it together
		- Looks similar to $l_{2}$ norm
## Eigenvalues and eigenvectors
- Important concept involving square matrices are eigen values and eigenvectors
- Non square matrices do not have eigen values or egein vectors
	- However they have singular values and singular vectors
- For square matrix $A$
	- $Av = \lambda v$
		- $v$ = eigen vectors of $A$
		- $\lambda$ = eigen values of $A$, which are numbers (scalar)
	- If $A$ is $n \times n$, there can be up to $n$ eigenvalues
	- Eigen values are typically written in non increasing order ($\leq$)
		- $\lambda_{1}$ is largest, $\lambda_{n}$ is smallest
- For any matrix $A$
	- $\sigma_{i}(A) = \sqrt{ \lambda_{i}(A^TA) } = \sqrt{ \lambda_{i} (AA^T) }$
		- $i$-th singular value (SV) of $A$
- Singular value decomposition (SVD)
	- $A = U \Sigma V^T$
		- $\Sigma =$ diagonal matrix
			- All roles not along diagonal are 0
				- $\Sigma_{ii} = \sigma_{i}(A)$
				- Diagonal entries are equal to the $i$-th singular value of $A$
		- $U, V$ are orthogonal
			- $U^T U = U U^T = I$
- Spectral norm
	- $\lVert A \rVert_{2} = \sigma_{i}(A)$
	- Equal to the largest singular value of $A$
	- Subscript 2 is normal way to write spectral norm
# Vector calculus review
## Gradients and Hessians
- Gradient -> For functions with vector inputs and scalar outputs
	- The column vector of partial derivatives of the function with respect to each entry in the input is the **gradient**
	- Let $f : \mathbb{R}^n \to \mathbb{R}$ be a scalar field
		- This means $f$ maps some vector of inputs in $\mathbb{R}^n$ dimensions to a scalar output (real numbers $\mathbb{R}$)
	- The gradient of $f$ is denoted as $\nabla f$
		- $\nabla f : \mathbb{R}^n \to \mathbb{R}^n$
			- $f(x_{1}, x_{2}, \dots, x_{n})$
			- $$\large \nabla f(x_{1}, x_{2}, \dots x_{n}) = \begin{bmatrix} \dfrac{ \partial f }{ \partial x_{1} } \\ \dfrac{ \partial f }{ \partial x_{2} } \\ \vdots \\ \dfrac{ \partial f }{ \partial x_{n} } \end{bmatrix} = \begin{bmatrix} f_{x_{1}}(x_{1}, x_{2}, \dots, x_{n}) \\ f_{x_{2}}(x_{1}, x_{2}, \dots, x_{n}) \\ \vdots \\ f_{x_{n}}(x_{1}, x_{2}, \dots, x_{n}) \end{bmatrix} = \begin{bmatrix}f_{x_{1}} \\ f_{x_{2}} \\ \vdots \\ f_{x_{n}}\end{bmatrix}$$
				- $f_{x_{1}}$ here means the partial derivative of $f$ with respect to $x_{1}$
				- $(\nabla f)_{j} = \dfrac{ \partial f }{ \partial x_{j} }$
			- $\nabla f$ is a vector field, because it maps a vector of inputs to a vector output
	- Commonly, people write $\dfrac{df}{dx} = (\nabla_{x} f)^T$
- Jacobian -> For functions with vector inputs and **vector outputs**
	- Then we have a matrix of partial derivatives of partial derivatives which is referred to as **Jacobian** matrix
	- Each cell is the partial derivative of a specific input variable over a specific output variable
	- $F : \mathbb{R}^n \to \mathbb{R}^m$
		- Let $F$ be a vector field
		- Jacobian matrix is considered a derivative over the vector field
		- Jacobian matrix is denoted as $J$
	- $i^\text{th}$ row is the gradient of the $i^\text{th}$ component of $F$
	- $$\huge \begin{align}
		&F = \begin{bmatrix}
		f_{1}(x_{1}, x_{2}, \dots x_{n}) \\ f_{2}(x_{1}, x_{2}, \dots x_{n}) \\ \vdots \\ f_{n}(x_{1}, x_{2}, \dots x_{n}))
		\end{bmatrix} \\ \\
		&J(F) = \begin{bmatrix} f_{1x_{1}} &f_{1x_{2}} &\dots &f_{1x_{n}} \\ f_{2x_{1}} &f_{2x_{2}} &\dots &f_{2}x_{n} \\ \vdots &\vdots &\ddots &\vdots \\ f_{n x_{1}} &f_{n x_{2}} &\dots &f_{n x_{n}} \end{bmatrix}
		\end{align}$$
		- $J_{i,j} = \dfrac{ \partial f_{i} }{ \partial x_{j} }$
		- Notice how the gradient looks like the $F$, matrix.
		- If we take the Jacobian of the gradient, we get the hessian matrix
- Hessian matrix
	- Matrix of all partial second derivatives for a vector-input scalar-output function
	- AKA the **Jacobian** of the gradient function
	- Hessian matrix is denoted as $H_{i,j}$
	- $$\huge H(f) = J(\nabla f) = \nabla^2 f = \begin{bmatrix} f_{x_{1}x_{1}} &f_{x_{1}x_{2}} &\dots &f_{x_{1}x_{n}} \\ f_{x_{2}x_{1}} &f_{x_{2}x_{2}} &\dots & f_{x_{2}x_{n}}\\ \vdots &\vdots &\ddots &\vdots \\ f_{x_{n} x_{1}} &f_{x_{n} x_{2}} &\dots &f_{x_{n} x_{n}} \end{bmatrix}$$
		- This matrix is square and symmetric
		- For convex functions, its Hessian is positive semidefinite
	- $H_{i,j} = \dfrac{ \partial^2 f }{ \partial x_{i} \partial x_{j} }$
		- This is a matrix of second-ordered mixed partials of a scalar field
		- [Mixed partial derivatives](https://tutorial.math.lamar.edu/classes/calciii/highorderpartialderivs.aspx)
	- Note, this is not the same as entropy (even though they both are denoted using $H$)
# [Matrix cookbook](https://math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf) ^matrix-cookbook
