# 5 Backpropagation
# Neural Networks
## Drawing computation graphs
- Computation graph
	- Graph visualizing flow of computation
- ![[Pasted image 20250403232241.png]]
- What expression does this compute?
	- $$\lVert (x_{1} \theta_{1} + x_{2} + \theta_{2}) - y \rVert^2$$
	- This is the MSE loss with a linear regression model
		- When we want to compute the gradients, we use the computation graphs
- Neural networks are computation graphs
- If we design generic tools for computation graphs, we can train many kinds of neural networks
- Let's use vectors now to represent weights, inputs, and outputs
	- Multiplication this time means dot product
	- ![[Pasted image 20250403232432.png]]
		- Linear regression computation graph
## Logistic regression
- Algorithm for classification
- Loss -> negative log likelihood
	- $\huge p_{\theta}(y \mid x) = \dfrac{\exp(x^T \theta_{y})}{\sum_{y'} \exp(x^T \theta_{y'})}$
- $y$ is a "one-hot" vector
	- Contains zero everywhere, except for entry that corresponds to the true label
	- If we have two labels, and the ground truth is the first label, then our vector looks like
		- $\begin{bmatrix}1 \\ 0\end{bmatrix}$
	- If we have two labels, and the ground truth is the second label, then our vector looks like
		- $\begin{bmatrix}0 \\ 1\end{bmatrix}$
- We can use the $y$ "one-hot" vector as a selector to get the first term of the negative log likelihood
	- ![[Pasted image 20250403232952.png]]
- Finally we can compute the second part and then we subtract the two
	- ![[Pasted image 20250403233124.png]]  
## Logistic regression
- Simpler way to draw the same thing
	- ![[Pasted image 20250403233637.png]]
	- ![[Pasted image 20250403233259.png]]
- Can write computation graph much more easily in matrix notation
- Matrix $\theta$
	- Number of rows = number of labels
	- Number of columns = dimension of $x$
- ![[Pasted image 20250403233621.png]]
- Explanation
	- Multiply matrix $\theta$ with vector $x$
	- Then feed it to softmax
		- Outcome of softmax
	- We can then use the "one-hot" vector $y$ to 'select" the probability of true label
## Draw it even more concisely
- Notice we have two types of variables
	- Data ($x$, $y$) which serves as input or target output
	- Parameters (**Ex.** $\theta$)
		- Parameters **usually affect one specific operation**
		- There is often parameter sharing (see conv nets later)
- Can summarize computation graph even more concisely
	- ![[Pasted image 20250403233952.png]]
	- Explanation
		- Linear layer
			- Sometimes called **fully connected layer**
		- Cross-entropy loss (another name of negative log likelihood)
			- Loss always depends on $y$, therefore we can omit the $y$ node
			- Nothing else can depend on $y$ (otherwise we're cheating and looking at the answer to come up with our response)
## Neural network diagrams
- Simplified computation graph diagram
	- ![[Pasted image 20250403234113.png]]
- Neural network diagram
	- ![[Pasted image 20250403234346.png]]
	- Visualize variables as boxes
	- Linear layers are often represented as trapezoid
		- Technically depends on $\theta$, but it's often omitted because the layers' parameters are often not shared with anything else
	- Linear layer transforms $x$ into intermediate representation $z^{(1)}$
	- We often don't draw cross-entropy box, because cross entropy loss always follows softmax
- Simplified drawing
	- ![[Pasted image 20250403234359.png]]
## Logistic regression with features
- Linear model works great
	- ![[Pasted image 20250403234600.png]]
- What about checkerboard pattern?
	- ![[Pasted image 20250403234609.png]]
	- However we could draw the line if we used feature
	- We add new features that augment inputs
		- **Ex.** $x^2$
	- ![[Pasted image 20250403234617.png]]
		- Now we feed the input $x$ into $\phi(x)$ to process it
## Learning the features
- Problem -> how to represent learned features?
- Idea -> what if each feature is a (binary) logistic regression output?
- For a specific feature $\phi_{1}$
	- ![[Pasted image 20250403234922.png]]
		- This softmax function for the binary classification is also known as the **sigmoid** function
			- ![[Pasted image 20250403235111.png]]
		- Will switch to using lowercase $w$ or $W$ instead of $\theta$ here
		- $\theta =$ all parameters of the model
		- $w_{1}^{(1)} =$ weights (parameters) of feature $1$ at layer $1$
			- ![[Pasted image 20250403235025.png]] ![[Pasted image 20250403235036.png]]
- We a general feature function that takes in the input $x$, and can output multiple features in a vector
- ![[Pasted image 20250403235120.png]]
	- We can simplify this vector of features into $\sigma(W^{(1) x}$
	- $\sigma =$ per-element sigmoid
		- **NOT** the same as the softmax
		- Each feature is independent
			- Every dimension uses the same $\text{softmax}$ function
- Now let's draw this out!
	- ![[Pasted image 20250403235653.png]]
	- ![[Pasted image 20250403235759.png]]
- Simpler way to draw the same thing
	- ![[Pasted image 20250403235846.png]]
	- "Sigmoid" layer
		- Linear operation followed by sigmoid
- Even simpler
	- ![[Pasted image 20250403235904.png]]
## Doing it multiple times
- ![[Pasted image 20250403235945.png]]
- ![[Pasted image 20250403235950.png]]
## Activation functions
- ![[Pasted image 20250404000030.png]]
- We don't have to use a sigmoid!
- Wide range of non-linear functions will work
- These functions are called **activation functions**
	- Why **non-linear**?
- Consider the identity linear function (outputs input exactly)
	- $a^{(2)} = \sigma(W^{(2)} \sigma(W^{(1)} x))$
	- $\sigma(z) = z$, then,
		- $a^{(2)} = W^{(2)} W^{(1)} x = Mx$
		- We can simplify the two layers down into a matrix that then is essentially one layer
		- Multiple linear layers = one linear layer
- Enough layers = we can represent anything (so long as they're nonlinear)
	- ![[Pasted image 20250404000340.png]]
	- ![[Pasted image 20250404000345.png]]
## Demo
![[Pasted image 20250404000537.png]]
- Even if we don't design the features ourselves, using intermediate layers can let the neural network built its own features
## Aside: what's so neural about it?
- ![[Pasted image 20250404000700.png]]
	- Dendrites receive signals from other neurons
	- Neuron "decides" whether to fire based on incoming signals
	- Axon transmits signal to downstream neurons
- ![[Pasted image 20250404000824.png]]
	- Artificial "neuron" sums up signals from upstream neurons (also referred to as "units)
	- $$z = \sum_{i} a_{i}$$
		- $a_{i} =$ upstream activations
		- Neuron "decides" how much to fire based on incoming signals
			- $a = \sigma(z)$
			- That's why $\sigma$ is called the activation function
# Training neural networks
## What do we need?
1. Define model class -> neural network
2. Define loss function -> negative log-likelihood
3. Pick optimizer -> stochastic gradient descent
	- What do we need to apply SGD?
	- We need to calculate the gradient $\nabla_{\theta} \mathcal{L}(\theta)$
	- ![[Pasted image 20250404000947.png]]
4. Run on big GPU
## Aside: chain rule
- Chain rule
	- ![[Pasted image 20250404001236.png]]
	- $$\Huge \frac{d}{dx} f(g(x)) = \frac{dz}{dx} = \underbrace{ \frac{dy}{dx} }_{ \text{Jacobian of g} } \cdot \underbrace{ \frac{dz}{dy} }_{ \text{Jacobian of f} }$$
- Same rule applies to multivariable
- High-dimensional chain rule
	- $$\huge \frac{d}{dx_{i}} f(g(x)) = \sum_{j=1}^m \frac{dy_{j}}{dx_{i}} \frac{dz}{dy_{j}} = \underbrace{ \frac{dy}{dx_{i}} }_{ \text{row } 1 \times m } \cdot \underbrace{ \frac{dz}{dy} }_{ \text{col } m \times 1 }$$
	- Let's compute derivative of $z$ with respect to $x_{i}$
		- We can break this down into a sum over all dimensions of $y$
	- This is just dot product between $\dfrac{d}{dx_{i}}$ and $\dfrac{dz}{dy}$
	- $$\huge \frac{d}{dx} f(g(x)) = \underbrace{ \frac{dy}{dx} }_{ \text{mat } n \times m } \cdot \underbrace{ \frac{dz}{dy} }_{ \text{col } m \times 1 }$$
- Row or column?
	- Expressing derivatives of functions with vector inputs as column vectors or row vectors?
	- In this lecture:
		- ![[Pasted image 20250404002252.png]]
			- Within the matrix $\huge \dfrac{dy}{dx}$
				- For each entry $\left(\dfrac{dy}{dx}\right)_{ij}$
					- First index of entry $i$ is denominator
					- Second entry $j$ is numerator
			- This is useful because we can store derivatives in vectors and matrices with same dimensionality of weight vectors and weight matrices
	- In some textbooks
		- ![[Pasted image 20250404002346.png]]
## Chain rule for neural networks
- Network network is just composition of functions
- Can use chain rule to compute gradients!
	- ![[Pasted image 20250404003228.png]]
	- Notice how we chain together derivatives!
## Does it work?
- ![[Pasted image 20250404003420.png]]
- We **can** calculate each of these Jacobians
- Example
	- ![[Pasted image 20250404003427.png]]
- Why might this be a bad idea?
	- If each $z^{(i)}$ or $a^{(i)}$ has about $n$ dimensions,
		- Each Jacobian is about $n \times n$ dimensions
		- Matrix multiplication is $O(n^3)$
- Do we care?
	- Minimum performance is $O(n^2)$ because we store weights in matrices
	- AlexNet has layers with 4096 units
		- Therefore $O(n^3)$ adds $n = 4096$ of a factor to the runtime
## Do it more efficiently
- ![[Pasted image 20250404003655.png]]
	- Loss is always scalar-valued
- Idea -> start on the right
	- ![[Pasted image 20250404003732.png]]
	- ![[Pasted image 20250404003741.png]]
- By starting on the right, we keep operations $O(n^2)$ without having to deal with matrix multiplication
## The backpropagation algorithm
- "Classic" version
- ![[Pasted image 20250404003904.png]]
- Forward pass -> calculate each $a^{(i)}$ and $z^{(i)}$
- Backward pass -> initialize $\delta = \dfrac{d\mathcal{L}}{dz^{n}}$
- For each $f$ with input $x_{f}$ and parameters $\theta_{f}$ from end to start
	- $\dfrac{d\mathcal{L}}{d\theta_{f}} \leftarrow \dfrac{df}{d\theta_{f}} \delta$
	- $\delta \leftarrow \dfrac{df}{dx_{f}}\delta$
## Walkthrough
![[Pasted image 20250404004706.png]]
# Neural network architecture details
- Some things we should figure out
	- How many layers?
	- How big are the layers?
	- What type of activation function?
- $\sigma(x) = \dfrac{1}{1 + \exp(- x)}$
	- ![[Pasted image 20250404005014.png]]
- $\text{ReLU}(x) = \max(0, x)$
	- ![[Pasted image 20250404004959.png]]
	- Benefits
		- Computationally cheap to implement
		- Derivatives are easy to calculate
		- Most popular activation function
## Bias terms
- Linear layer
- $z^{(i + 1)} = W^{(i)} a^{(i)}$
	- Problem -> if $a^{(i)} = \vec{0}$, we always get $0$
- Solution -> add a "bias"
	- $z^{(i + 1)} = W^{(i)} a^{(i)} + b^{(i)}$
		- Commonly called an affine layer
		- $b^{(i)}$ are now additional parameters in the neural network
	- **NOTE:** This has nothing to do with the bias/variance bias
## What else do we need for backprop?
- 