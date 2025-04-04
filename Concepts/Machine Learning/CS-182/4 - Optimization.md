# 4 Optimization
## The loss "landscape"
- $$\Huge \theta^* \leftarrow \arg \underset{ \theta }{ \min } \underbrace{ - \sum_{i} \log p_{\theta}(y_{i} \mid x_{i})B }_{ \mathcal{L}(\theta) }$$
- Lets say $\theta$ is 2D
- $\mathcal{L}(\theta)$ is our loss function
	- Assume we are using the [[2 - Machine Learning Basics#^negative-log-likelihood|negative log likelihood]] loss function
- Algorithm
	1. Find direction $v$ where $\mathcal{L}(\theta)$ decreases (not that v is a vector)
	2. $\theta \leftarrow \theta + \alpha v$
		1. $\alpha =$ some small constant called "learning rate" or "step size"
		2. If it's too big, it can overshoot
		3. Repeat
## Gradient Descent
- Which way does $\mathcal{L}(\theta)$ decrease?
	- ![[Pasted image 20250403211557.png]]
- Negative slope -> go to right
- Positive slope -> go to left
- In general
	- For each dimension, go in the direction opposite to the slope along that dimension
	- For each dimension, we take partial derivative of loss function with regard to that dimension
		- $v_{1} = - \dfrac{ \partial \mathcal{L}(\theta) }{ \partial \theta_{1} }$
		- $v_{2} = - \dfrac{ \partial \mathcal{L}(\theta) }{ \partial \theta_{2} }$
- We can stack these partial derivatives into a vector, called a **gradient**
	- $\nabla_{\theta} \mathcal{L}(\theta) = \begin{bmatrix}\dfrac{ \partial \mathcal{L}(\theta) }{ \partial \theta_{1} } \\ \dfrac{ \partial \mathcal{L}(\theta) }{ \partial \theta_{2} } \\ \vdots \\ \dfrac{ \partial \mathcal{L}(\theta) }{ \partial \theta_{n} }\end{bmatrix}$
		- Dimension of this vector is same as # of parameters $\theta$
	- We can then set $v = - \nabla_{\theta} \mathcal{L}(\theta)$
		- We set our direction to the negative of the gradient
## Visualizing gradient descent
![[Pasted image 20250403211956.png]]
- Contour plot
	- Lines represent level-set contours
		- Similar to an elevation map
		- For all $\theta$ along the same line, the loss $\mathcal{L}(\theta)$ is the same
	- The step is the alpha times the gradient
	- $x$ and $y$ axes represent two parameters, $\theta_{1}$ and $\theta_{2}$
- Challenges to gradient descent
	- Oscillation
	- We don't get all the way to the optimum, we get slower and slower
## What's going on?
- We don't always moves toward the optimum!
- We take the steepest descent direction
	- Not necessarily towards the minimum point!
	- ![[Pasted image 20250403212416.png]]
- Direction of steepest descent is always tangent to contour
- Given infinite gradient steps, we may eventually reach the optimum
	- ![[Pasted image 20250403212513.png]]
## The loss surface
- This loss surface is nice
	- ![[Pasted image 20250403212549.png]]
	- No narrow, steep valleys
		- No matter what you take, you can reach the optimum location -> "all roads lead to Rome"
- Is loss function nice for logistic regression?
	- $$p_{\theta}(y = i \mid x) = \frac{\exp(x^T \theta_{i})}{\sum_{j=1}^m \exp(x^T \theta_{j})}$$
	- Negative likelihood loss for logistic regression is guaranteed to be convex
		- This is not an obvious or trivial statement, but can be proven
- **DEFINE:** Convexity
	- A function is convex if a line segment between any two points lies "above" the graph
	- Convex
		- ![[Pasted image 20250403212821.png]]
		- Implies there is one optimum
		- Convex function are "nice" because simple algorithms like gradient descent have strong guarantees
			- We will eventually find the global optimum
			- Doesn't mean gradient descent works well for all convex functions
				- Might take a long time
	- Not convex
		- Can have one or more optimum
		- ![[Pasted image 20250403212828.png]]
		- ![[Pasted image 20250403212845.png]]
# The loss surface of a neural network
![[Pasted image 20250403213000.png]]
- Pretty hard to visualize loss surface of neural networks, because they have large numbers of parameters
- Let's give it a try
	- ![[Pasted image 20250403213157.png]]
	- This does **NOT** look nice
	- Global optimum is at bottom
	- Many plateaus, and local optimum that we can get stuck on
- Some networks are better!
	- ![[Pasted image 20250403213209.png]]
	- Doesn't have as many nooks and crannies
## Geography of loss landscape
- Local optimum
- Plateau
- Saddle
## Local Optima
- ![[Pasted image 20250403213311.png]]
- More than one point where derivative is zero
	- Most obvious issue with non-convex loss landscapes
	- One of the big reasons people used to worry about neural networks!
- Very scary in principle, since gradient descent could converge to solution that is worse than global optimum, but we have no way of knowing!
	- Derivatives still look like all zero
- Surprisingly, this becomes less of an issue as number of parameters increases!
	- "The loss surface of multilayer networks" paper
	- For big networks, local optima exist, but tend to not be much worse than global optima
	- ![[Pasted image 20250403213537.png]]
	- Plotting histogram of loss
		- Horizontal axis -> loss
		- Vertical axis -> # rounds of gradient descent required to achieve the loss
	- Colors -> number of parameters in network 
	- Larger networks correspond to bigger parameters
	- Bigger the network gets, the distribution of solutions becomes tighter, and more clustered
## Plateaus
- ![[Pasted image 20250403214013.png]]
- Can't just choose tiny learning rates to prevent oscillation
	- Tiny learning rate will take forever
	- Need a learning rate large enough to not get stuck in the plataeu
## Saddle points
- ![[Pasted image 20250403214108.png]]
- Exists for higher dimensional functions (2D, 3D)
- Point where local minimum along some direction, but a local maximum among other directions
- Partial derivative (gradient) is still zero at saddle point
	- At the saddle point, we're taking very small steps
	- Green line in third diagram shows the direction we take for gradient descent
		- Notices how it slows down as it does a "drive by" on the saddle
- This seems like a very special structure, does it happen that often?
	- Yes! In fact, most critical points in neural net loss landscapes are saddle points
- Critical points
	- Any point where $\nabla_{\theta} \mathcal{L}(\theta) = 0$
	- ![[Pasted image 20250403214833.png]]
	- Is it a maximum, minimum, or saddle?
		- Check second derivative
			- Positive -> local minimum
			- Negative -> local maximum
- Higher dimensions -> Hessian matrix
	- "Second derivative equivalent" of the gradient
		- Matrix where every entry is second derivative of two entries of $\theta$
	- ![[Pasted image 20250403214924.png]]
	- At the saddle point, the Hessian matrix will have diagonal entries $\pm 1$
		- **Ex.**
			- ![[Pasted image 20250403215033.png]]
		- This is only a maximum or minimum if **ALL** of the diagonal entries are positive or negative!
			- For non diagonal matrices -> we want it to be positive definite or negative definite
			- **OTHERWISE** it's a saddle!
		- How often is that the case?
			- Seems very unlikely for every entry to be positive or every entry to be negative!
			- Therefore, **MOST** critical points are saddle points in higher dimensions!
## Which way do we go?
- Instead of wiggling back and forth, could we take a more direct path?
	- ![[Pasted image 20250403215415.png]]
# Improvement directions
- Can we find this direction?
	- Yes, with Newton's method!
	- We won't use Newton's method for neural networks
		- Too expensive, requires too much computatikon
- But it's an "ideal" to aspire to, compared to direction of steepest descent
## Netwon's method
- Taylor expansion (approximates value of a function using first derivative + second derivative + third derivative ...)
- $$f(x) \approx f'(x_{0})(x - x_{0}) + \frac{1}{2} f''(x_{0})(x - x_{0})^2$$
	- ![[Pasted image 20250403215644.png]]
	- Essentially approximating the function as a parabola
- Multivariate case
	- ![[Pasted image 20250403220023.png]]
	- We can optimize this analytically
	- Set derivative to zero and solve, and we get
	- $$\huge \theta^* \leftarrow \theta_{0} - (\nabla_{\theta}^2 \mathcal{L}(\theta_{0}))^{-1} \nabla_{\theta} \mathcal{L}(\theta_{0})$$
		- **NOTE:** This looks a lot like gradient descent
## Tractable acceleration
- Why is Newton's method not a viable way to improve neural network optimization?
	- Gradient descent
		- ![[Pasted image 20250403220241.png]]
		- Runtime -> $O(n)$
			- Assume it takes constant time for each partial derivative (in reality we use back propagation) 
	- Netwon's method -> uses hessian
		- ![[Pasted image 20250403220234.png]]
		- Runtime -> $O(n^2)$
			- Hessian matrix itself takes $O(n^2)$
			- Calculating inverse multiplies by $O(n)$
			- Only if using naive approach
				- However, fancy methods can be much faster if they avoid forming the Hessian explicitly
				- But generally, they end up be very complicated
- Since Hessian calculations are so expensive, we prefer methods that don't require second derivatives, but "accelerate" gradient descent instead
## Momentum
- What if we average the update a few update steps?
	- ![[Pasted image 20250403220721.png]]
	- Averaging together successive gradients seems to yield a much better direction!
- Intuition
	- If successive gradient steps point in different directions, cancel off directions that disagree
		- ![[Pasted image 20250403220757.png]]
	- If successive gradient steps point in similar directions, we should **go faster** in that direction
		- ![[Pasted image 20250403220828.png]]
## Momentum
- Update rule
	- $\theta_{k+1} = \theta_{k} - \alpha g_{k}$
- $g_{k}$ is now our improved direction
	- Before -> $g_{k} = \nabla_{\theta}\mathcal{L}(\theta_{k})$
		- Direction was just gradient
	- Now -> $g_{k} = \mathcal{L}(\theta_{k}) + \mu g_{k - 1}$
		- "Blend in" the previous direction to the gradient
		- Multiply the previous direction by a "decay factor" $\mu$
			- Common choice 0.99
- Very simple update rule
- In practice, brings some of the benefits of Newton's method, at virtually no cost
- This kind of momentum method has few guarantees
- Closely related idea -> "Nesterov accelerated gradient"
	- This algorithm **DOES** carry very appealing guarantees
	- In practice we just use momentum
## Momentum Demo
- ![[Pasted image 20250403221546.png]]
- ![[Pasted image 20250403221603.png]]
	- Zig zags due to "momentum" from previous gradients
	- However, it slides along bottom of valley more effectively (less oscillations in general)
## Gradient scale
- ![[Pasted image 20250403221920.png]]
- Intuition -> Sign of gradient tells us way to go along each dimension
	- Magnitude is **not as useful**
- Even worse -> overall magnitude of gradient can change drastically over the course of the optimization, making learning rates hard to tune
	- ![[Pasted image 20250403222027.png]]
	- If the difference, $f_{\theta}(x) - y$ is huge, then we are taking **huge steps** when we're far away from the optimum
- Idea -> "normalize" out the magnitude of the gradient along each dimension
## Algorithm: RMSProp
- "Root mean squared" propagation
- Idea -> estimate per-dimension magnitude (running-average) of gradient
	- $\huge s_{k} \leftarrow \beta s_{k-1} + (1 - \beta)(\nabla_{\theta} \mathcal{L}(\theta_{k}))^2$
	- $s_{k}$ is our new direction
		- Has same number of entries as gradient
		- Roughly the squared length of each dimension
- $\huge \theta_{k+1} = \theta_{k} - \alpha \nabla_{\theta} \dfrac{\mathcal{L}(\theta_{k})}{\sqrt{ s_{k} }}$
	- Each dimension is divided by roughly its magnitude
	- Doesn't have strong guarantees
## Algorithm: AdaGrad
- Provides more guarantees -> improvement over RMSProp
	- Similar idea to how "Nesterov accelerated gradient" improves the momentum algorithm
- Estimate per-dimension **cumulative** magnitude
	- $\huge s_k \leftarrow s_{k-1} + (\nabla_{\theta} \mathcal{L}(\theta_{k}))^2$
		- We remove the $\beta$ factor from RMSProp
	- $\huge \theta_{k + 1} = \theta_{k} - \alpha \nabla_{\theta} \dfrac{\mathcal{L}(\theta k)}{\sqrt{ s_{k} }}$
- How does AdaGrad and RMSProp compare?
	- AdaGrad has some appealing guarantees for convex problems
		- Learning rate effectively "decreases" over time, which is good for convex problems
			- $s_{k}$ grows bigger and bigger over time
			- Since we divide by $s_{k}$, this means our updates get tinnier
			- We must find optimum quickly before the learning rate decays too much!
		- RMSProp tends to be much better for deep learning and non-convex problems
			- In non convex problems, we might hit a plateau, which might kill our progress
		- RMSProp forgets (old values slowly decay out), but Adagrad never forgets
## Algorithm: Adam
- Basic idea -> combine momentum and RMSProp ("normalization")
- $\huge m_{k} = (1 - \beta_{1}) \nabla_{\theta} \mathcal{L}(\theta_{k}) + \beta_{1}m_{k-1}$
	- First moment estimate
		- Similar to momentum, adding the previous direction
		- Running average of gradient
	- $m$ stands for **mean**
- $\huge v_{k} = (1 - \beta_{2})(\nabla_{\theta}\mathcal{L}(\theta_{k}))^2 + \beta_{2}v_{k-1}$
	- Second moment estimate
		- Running average of squared lengths
		- A lot like $s_{k}$ vector in RMSProp
	- $v$ stands for **variance**
- $\huge \hat{m_{k}} = \dfrac{m_{k}}{1 - \beta_{1}^k}$
	- Why?
		- When we just start off, $m_{0} = v_{0} = 0$
			- $\beta$ tends to be large numbers
				- Therefore, $m_{1}$ would be the first term + 0
				- Since first term is $1 - \beta$, this means first term is very small
				- Therefore we take very tiny steps at beginning
		- Therefore by dividing by $1 - \beta_{1}^k$, we cancel out the effect of $(1 -\beta_{1})$ in the RHS of the $m_{1}$
			- $\huge m_{1} = \dfrac{m_{1}}{1 - \beta_{1}^1} = \dfrac{(1 - \beta_{1}) \nabla_{\theta}\mathcal{L}(\theta_{k})}{(1-\beta_{1})} = \nabla_{\theta} \mathcal{L}(\theta_{k})$
			- $\huge m_{2} = \dfrac{m_{2}}{1 - \beta_{1}^2} = \dfrac{(1 - \beta_{1}) \nabla_{\theta}\mathcal{L}(\theta_{k})}{(1-\beta_{1}^2)}$
			- Note that as we get 
- $\huge \hat{v}_{k} = \dfrac{v_{k}}{1 - \beta_{2}^k}$
- $\theta_{k + 1} = \theta_{k} - \alpha \dfrac{\hat{m}_{k}}{\sqrt{ \hat{v}_{k} } + \epsilon}$
	- $\epsilon = 10^{-8}$
	- Epsilon is a small number to prevent divide by zero when $\hat{v}_{k} = 0$
- Good default settings
	- $\alpha = 0.001$
	- $\beta_{1} = 0.9$
		- Represents direction
		- Direction can change a lot
		- Smaller $\beta_{1}$, because we don't want the direction to change too much, otherwise we'd be taking average of directions all over the place
	- $\beta_{2} = 0.999$
		- Represents magnitude
## Why is gradient descent expensive?
- All versions described so far are not suitable for depe learning
- $$\huge \mathcal{L}(\theta) = \dfrac{1}{N} \sum_{i = 1}^N \log p_{\theta}(y_{i} \mid x_{i})$$
	- Requires summing over **all** datapoints in the dataset
	- This is not feasible for large datasets
	- ![[Pasted image 20250403225742.png]]
- However this "sum" is just **a sampled-based estimate** of the expected value
	- $$\huge \mathcal{L}(\theta) = \dfrac{1}{N} \sum_{i = 1}^N \log p_{\theta}(y_{i} \mid x_{i}) \approx -E_{p_{\text{data}}(x, y)}[\log p_{\theta}(y_{i} \mid x_{i})]$$
	- Sample based estimate is unbiased
		- Repeating process many times, on average, we get the right answer
	- We can therefore use fewer samples, and still have a correct (unbiased) estimator
	- $$\huge \mathcal{L}(\theta) \approx -E_{p_{\text{data}}(x, y)}[\log p_{\theta}(y_{i} \mid x_{i})] \approx - \frac{1}{B} \sum_{j = 1}^B \log p_{\theta}(y_{i_{j}} \mid x_{i_{j}})$$
		- $B \ll N$
			- $B$ is **much smaller** than $N$
		- Our loss function will now have some **noise** depending on what samples we use
			- However overall, it's centered on correct expected value
## Stochastic gradient descent (SGD)
- With minibatches
	1. Sample $\mathcal{B} \subset \mathcal{D}$
		1. Draw $B$ datapoints at random from dataset of size $N$
	2. Estimate $\huge g_{k} \leftarrow - \nabla_{\theta} \frac{1}{B} \sum_{i = 1}^B \log p(y_{i} \mid x_{i}, \theta) \approx \nabla_{\theta} \mathcal{L}(\theta)$
		2. Estimate gradient using just a few samples (total $B$ samples)
	3. $\huge \theta_{k + 1} \leftarrow \theta_{k} - \alpha g_{k}$
		1. Apply gradient descent on the approximate gradient
		2. Can use momentum, ADAM, etc, in place of $g_{k}$
- Each iteration samples a different **minibatch**
	- Every datapoint is used at some point
- Stochastic gradient descent in practice
	- Sampling randomly is slow due to random memory access
	- Instead, shuffle the dataset (like a deck of cards) once, in advance
	- Then index dataset sequentially, constructing batches out of consecutive groups of $B$ datapoints
		- ![[Pasted image 20250403230725.png]]
		- Order is still random, performs just as well as true stochastic gradient descent
## Learning rates
- Vertical -> training loss
- Horizontal -> epoch
	- **DEFINE:** Epoch
		- How many times we've gone through the whole dataset
		- 1 epoch is one cycle through the entire dataset
			- ![[Pasted image 20250403230921.png]]
- ![[Pasted image 20250403231258.png]]
- Low learning rates (green)
	- Conventionally, low learning rates are okay if you optimize long enough
	- However in deep learning, low learning rates can result in convergence in worse values
		- Can get stuck in plateaus
	- ![[Pasted image 20250403231205.png]]
- High learning rates (red)
	- We overshoot the optimum
	- Loss is stuck at height we overshoot
	- ![[Pasted image 20250403231219.png]]
## Decaying learning rates
- AlexNet trained on ImageNet
	- ![[Pasted image 20250403231314.png]]
	- Learning rate is divided by 10 every once in a while
	- At the start, we improve rapidly but we plataeu
		- After learning rate gets decreased, we can go deeper in to the holes
- Learning rate decay schedules usually needed for best performance with SGD (+ momentum)
- Often not needed with ADAM
- Opinions differ
	- Some people think SGD + momentum is better than ADAM
	- But ADAM is easier to tune
## Tuning (stochastic) gradient descent
- Hyperparameters
	- Batch size $B$
		- Larger batches, less noisy gradients
		- Usually "safer" but more expensive
		- Smaller batch requires taking more steps
	- Learning rate: $\alpha$
		- Best to use biggest rate that still works (no oscillation), decay over time
	- Momentum: $\mu$
		- 0.99 is good
	- Adam parameters: $\beta_{1}, \beta_{2}$
		- Keep defaults (usually)
- What to tune hyperparameters on?
	- Technically, we want to tune these parameters on training loss, since it is a parameter of the optimization
	- In practice, this is often tuned on validation loss
	- Relationship between stochastic gradient and regularization is complex
		- Some people consider stochastic gradient itself to be a good regularizer
			- Therefore we should be looking at validation loss b/c it's being considered a regularizer
		- If we overfit, gradients looks different from different batches