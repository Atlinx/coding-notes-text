# 2 Machine Learning Basics
- How do we formulate machine learning problems?
# Different Types of Learning Problems
- Making predictions -> Supervised learning
	- "Ground truth" labels
	- **Ex.**
		- Linear regression, predict $y$ from $x$
		- Image classification, predict label from image
- Categorizing unlabeled data -> Unsupervised learning 
- Learning strategies for interacting with environments -> Reinforcement learning
	- Controlling agent that takes sequential actions
	- Agent
		- Output action
		- Observe result + reward signal
	- Agent learns from "positive/negative reinforcement"
## Supervised Learning
![[Pasted image 20250206190524.png]]
- Given $D = \{(x_{1}, y_{1}), \dots, (x_{n}, y_{n})\}$
	- Training dataset
	- $x$ -> input
	- $y$ -> true label that corresponds to input $x$
- Goal -> learn $f_{\theta}(x) \approx y$
- Use cases
	- Linear regression
	- Labelling images
- Questions to answer
	- How do we represent $f_{\theta}(x)$?
		- $f_{\theta}(x) = \theta_{1}x_{1} + \theta_{2}x_{2} + \theta_{3}$
			- Linear regression
			- For a given input $x$, if `below_line(x)` then -> output circle
				- else -> output cross
		- $f_{\theta}(x) = \theta_{1}x_{1} + \theta_{2}x_{2}^2 + \theta_{3}x_{3}^3$
			- Polynomial regression
		- Neural network
	- How do we measure difference between $f_{\theta}(x_{i})$ and $y_{i}$?
		- Need a way to evaluate if one set of $\theta$ is better than another set
		- $|| f_{\theta}(x_{i}) - y_{i} ||^2$
			- Squared error
		- $\delta(f_{\theta}(x_{i}) \neq y)$
			- Zero-one loss
			- 1 if predicted = $y$
			- 0 otherwise
		- Probability?
	- How do we find the best setting of $\theta$?
		- Random search
		- Gradient descent
		- Least squares
## Unsupervised Learning
- Using unlabeled data to create representations
- Generative modeling
	- ![[Pasted image 20250206190813.png]]
	- **Ex.** GAN, VAEs, pixel RNN
		- Constructs pictures of faces
	- Neural network was trained on unlabeled data
		- Don't need labels or categories
	- Produces images resembling the input distribution
	- Images provided to model came from an underlying distribution (**ex.** distribution of all human faces, of all dogs, etc.)
	- Need to acquire understanding of representation to successfully generate outputs
- Self-supervised representation learning
	- BERT -> Deletes words, and predicts what those words were
		- ![[Pasted image 20250206190923.png]]
	- Cropping images, and making prediction on where the images are located
	- Not outright used, but is useful for downstream applications
		- **Ex.** Machine translation
## Reinforcement Learning
![[Pasted image 20250206210243.png]]
- Choose $f_{\theta}(s_{t}) = a_{t}$
	- Our function takes in a state $s_{t}$  and outputs an action $a_{t}$
	- To maximize $\sum_{t=1}^H r(s_{t}, a_{t})$
- Actually subsumes (generalizes) supervised learning!
	- **Ex.** Large reward for getting prediction correct
- Supervised leaning -> get $f_{\theta}(x_{i})$ to match $y_{i}$
- Reinforcement learning -> get $f_{\theta}(s_{t})$ to maximize reward (could be anything)
	- **Ex.** training a dog
		- Actions -> muscle contractions
		- Observations -> sight, smell
		- Rewards -> food
	- **Ex.** Robot
		- Actions -> motor torque
		- Observations -> camera images
		- Rewards -> task success measure
	- **Ex.** Logistics management
		- Actions -> what to purchase
		- Observations -> inventory
		- Rewards -> profit
- Other applications
	- Education -> recommend which topic to study next
	- YouTube recommendations
	- Ad placement
	- Healthcare (recommending treatments)
# Supervised Learning
- Overwhelming majority of machine learning is supervised learning
	- Encompasses all prediction/recognition models trained from ground truth data
	- Multi-billion dollar/year industry
	- Simple basic principles
- Given $D = \{(x_{1}, y_{1}), (x_{2}, y_{2}), \dots\}$
	- Goal -> learn $f_{\theta(x)} \approx y$
- Predict -> based on ...
	- Category of object -> based on image
	- Sentence in French -> based on sentence in English (translation)
	- Presence of disease -> based on x-ray image
	- Text of a phrase -> based on audio utterance (transcription)
	- $y$ -> based on $x$
## Prediction is Difficult
- Sometimes, an input seems like it could be multiple possible types of outputs
- ![[Pasted image 20250206211232.png]]
	- Outputting a probability gives us flexibility to express what's going on in the image
## Predicting Probabilities
- Predicting probabilities makes more sense than predicting discrete labels
- It's easier to learn due to smoothness
	- Intuitively, we can't change a discrete label "a tiny bit" -> it's all or nothing
	- We can change a probability "a tiny bit"
- Given a training set $D$
	- Learn $p_{\theta}(y | x)$
		- Probability distribution of $y$ given $x$
		- We will be outputting probabilities
## Conditional Probabilities
- $x$ -> random variable representing input
	- Why is it a random variable?
	- We don't know what $x$ will we get
- $y$ -> random variable representing output
- $$\huge p(x, y) = p(x) p(y | x)$$
	- Probability of $x$ and $y$ = probability of $x$ times probability of $y$ given $x$
	- Chain rule
	- "Generative methods"
		- Learns about the distribution of $p(x, y)$ itself
		- If you can learn $p(x, y)$, you can recover $p(y | x)$ by reordering the formula
- $$\huge p(y|x) = \dfrac{p(x, y)}{p(x)}$$
	- Definition of conditional probability
	- "Discriminative learning"
		- Goal is to discriminate between different $y$

## How Do We Represent It?
![[Pasted image 20250206211919.png]]
- Output object probability instead of labels
	- Valid probability distribution
		- Outputs must be from 0 to 1
		- Outputs must sum to 1
	- **Ex.** Recognizing 10 digits -> 10 possible labels -> output 10 numbers
- Our new program should return the probabilities of $y$ given an input $x$

^representation
**How about?** ^22a7d8
$$
\huge
\begin{align}
p(y = \text{dog} | x) = x^T \theta_{\text{dog}} \\
p(y = \text{cat} | x) = x^T \theta_{\text{cat}}  \\
\vec{\theta} = \{ \theta_{\text{dog}}, \theta_{\text{cat}}\}
\end{align}
$$
- Drawback -> Doesn't sum to one

**How about?**
$$
\huge
\begin{align}
f_{\text{dog}}(x) = x^T \theta_{\text{dog}} \\
f_{\text{cat}}(x) = x^T \theta_{\text{cat}}  \\
\vec{\theta} = \{ \theta_{\text{dog}}, \theta_{\text{cat}}\} \\
p(y | x) = \text{softmax}(f_{\text{dog}}(x), f_{\text{cat}}(x))
\end{align}
$$
- $\text{softmax(x, y)}$ -> forces inputs to be positive and sum to $1$
	- Valid probability

> [!note]
> - We could use any function in place of $\text{softmax}$ that takes inputs and outputs probabilities that are positive and sum to 1. 
> - Ideally an one-to-one (injective) or onto (surjective) function
> 	- We still want to use the inputs somehow
> 	- Having a function that returns values without consideration of inputs is useless

- Why any function?
	- Function just needs to be general enough such that there exists a $\theta$ that provides the right answer
	- Therefore we don't need to be too careful with our choice of function
# How Do We Represent It?
- How to make a number $z$ positive?
	- $z^2$
	- $|z|$
	- $\text{max}(0, z)$
	- $\exp(z)$
		- Especially convenient because it's a bijection (one-to-one and onto)
		- Maps entire real number line to entire set of positive reals
		- But don't overthink it, any one of these would work
- How to make a bunch of numbers sum to $1$?
	- Normalize -> $z_{1} = \dfrac{z_{1}}{z_{1} + z_{2}}$
	- In general, $z_{1} = \dfrac{z_{1}}{\sum_{1}^n z_{i}}$
$$\huge
\begin{align}
\text{softmax}_{\text{dog}}(f_{\text{dog}}(x), f_{\text{cat}}(x)) = \frac{\exp(f_{\text{dog}}(x))}{\exp(f_{\text{dog}}(x)) + \exp(f_{\text{cat}}(x))}  \\
\text{softmax}_{\text{cat}}(f_{\text{dog}}(x), f_{\text{cat}}(x)) = \frac{\exp(f_{\text{cat}}(x))}{\exp(f_{\text{dog}}(x)) + \exp(f_{\text{cat}}(x))}
\end{align}
$$
# Softmax in General
- $N$ possible labels
- $p(y | x)$ -> vector with $N$ elements
- $f_{\theta}(x)$ -> vector-valued function with $N$ outputs
- $$\huge p(y = i|x) = \text{softmax}(f_{\theta}(x))[i] = \frac{\exp(f_{\theta,i}(x))}{\sum_{j=1}^N \exp(f_{\theta, j}(x))}$$
> [!note]
> $\text{softmax()}$ returns a vector, which is why in our definition of a single probability we index on $[i]$ to get the $i$th slot or the "$i$th probability."
# Illustration: 2D Case
- $\theta$ that is learned defines a line through the space
- **Ex.** Consider space of blue and red points, we want to learn $\theta$ that divides the two types of points
	- ![[Pasted image 20250206215151.png]]
	- Anything above the line has a larger $P(\text{red})$
	- Anything below the line has a larger $P(\text{blue})$
	- Anything on the line has $P(\text{red}) = P(\text{blue}) = 0.5$
- As $x^T\theta_{y}$ gets bigger $p(y | x)$ gets bigger
	- "As the inner product of a set of parameters $\theta$ corresponding to a particular label $y$ gets bigger, the probability of $y$ given $x$ gets bigger"
	- Think back to [[#^22a7d8|how we represent the probabilities of y given x]]
		- Each label $y$ has its own set of parameters $\theta_{y}$
>[!note]
>$$\huge x^T \theta_{y} = \theta^T_{y} x = \theta_{y} \cdot x$$
>- Both $\theta_{y}$ and $x$ are 1-D vectors of the same length.
>- It doesn't matter which vector we choose to transpose before multiplying.
>- This operation is also known as the **dot-product** of the two vectors, represented in notation as $\theta_{y} \cdot x$.
# Illustration: 1D Case
- Consider a single axis
- Split the axis somewhere -> Decision boundary
	- At the boundary, $P(\text{red}) = P(\text{blue}) = 0.5$
	- ![[Pasted image 20250206220133.png]]
	- The softmax is illustrated above as the black line in the diagram
$$
\huge \begin{align}
P(\text{red} | x) = \text{softmax}_{\text{red}}(\theta^T_{\text{red}}x) = \frac{e^{\theta_{\text{red}}^T x}}{e^{\theta_{\text{red}}^T x} + e^{\theta_{\text{blue}}^T x}} \\
P(\text{blue} | x) = \text{softmax}_{\text{blue}}(\theta^T_{\text{blue}}x) = \frac{e^{\theta_{\text{blue}}^T x}}{e^{\theta_{\text{red}}^T x} + e^{\theta_{\text{blue}}^T x}}
\end{align}
$$
- ![[Pasted image 20250206221011.png]]
	- **DEFINE:** Hardmax
		- $\text{max}_{y} \theta_{y}^T x$
		- A step-like function
		- Outputs $1$ for the label with the highest dot-product
		- Outputs $0$ for the remaining labels
	- As we scale $\theta$, the softmax function becomes sharper
		- At a really large $\theta$, it looks like the hardmax function
			- Output almost $1$ for label with highest dot-product
			- Outputs almost $0$ for remaining label
		- As $\theta$ gets smaller
# Loss Functions
- So far
	- ![[Pasted image 20250206222248.png]]
	- We have parameters $\theta$
	- How do we select $\vec{\theta}$?
## The Machine Learning Method
- For solving any problem ever
	1. Define your model class
		- How do you represent the "program"
			- We did this in the last section
		- Set of programs -> programs that can be represented using different parameters $\theta$ with your program
			- **Ex.** All linear programs, all 3rd degree polynomial programs, etc. 
	2. Define your **loss function**
		- Loss function
			- Measures how good a model from your model class is
	3. Pick your optimizer
		- Search the model class to find the model that minimizes the loss function
		- Identifies the "optimal" model with optimal parameters $\theta$
	4. Run it on a big GPU
## Aside: Marr's Levels of Analysis
- Marr -> well known neuroscientist
- Proposed method of thinking about cognition
- Pyramid
	- Computation -> "why?" -> loss function (minimize loss function)
		- What is the algorithm trying to achieve?
	- Algorithmic -> "what?" -> the model
		- What is representation of function?
		- What structure answers that why?
	- Implementation -> "how?" -> optimizer
		- How is that structure constructed to?
		- How you choose the "what" to answer the "why"
	- *Bonus* -> on which GPU?
		- More relevant for ML engineer
- Optimizer, model, and loss function tends to be separated
	- Don't need to worry about all of them at once
# How is the Dataset "Generated"?
- We think of the dataset as a random distribution $p(x)$
- $$\huge \text{photograph} \sim p(x)$$
> [!note] Terminology
> $a \sim p(b)$ means a data-point $a$ is sampled from the probability distribution of $p(b)$
- $p(x) =$ Probability distribution of photos
- $\text{``dog"} \sim p(y | x)$
	- The label "dog" is sampled from some distribution of labels
		- This distribution of labels is the probability of getting the label $y$ given a photo $x$
		- Conditional probability distribution over labels
	- $p(y|x)$ -> true distribution of the real world
- Result -> $(x,y) \sim p(y|x) p(x) = p(x, y)$
	- Using chain rule in probability, $p(x,y) = p(y|x) p(x)$
	- Therefore, we can generate samples $(x,y)$ from the join distribution $p(x, y)$
- Training set $D = \{(x_{1}, y_{1}), (x_{2}, y_{2}), \dots\}$
- What is $p(D)$, the probability of getting our dataset?
- **Key assumption** -> our data is independent and identically distributed (i.i.d)
	- Independent -> every data-point $(x_{i}, y_{i})$ independent of each $(x_{j}, y_{j})$
	- Identically distributed -> $(x_{i}, y_{i}) \sim p(x,y)$
		- "All data-points $(x_{i}, y_{i})$ come from the same probability distribution of $p(x, y)$"
- When $\text{i.i.d.:}$
	- $$\huge p(D) = \prod_{i}p(x_{i}, y_{i}) = \prod_{i} p(x_{i}) p(y_{i} | x_{i})$$
	- Probability of getting our data set $D$ is the probability of getting each point $p(x_{i}, y_{i})$ multiplied together
	- We can do this because we assume the each data point is independent from one another, and share the same probability distribution

>[!note] Terminology
>$p(x_{i})$ means probability of getting a specific input $x_{i}$

- We are learning $p_{\theta}(y | x)$
	- Remember our [[#^22a7d8|model outputs probabilities of y given x]]
		- **Ex.** $p(y = \text{``dog"} | x)$, $p(y = \text{``cat"} | x)$
	- $p_{\theta}(y | x)$ is a "model" of the true $p(y | x)$
	- A good model should make the data look probable
		- If we randomly sampled the set of all possible data-points to form our data-set, the probability of getting our specific data-set should be relatively high 
		- Data-points that appear more often in the population should appear more often in our sample
			- It would be extremely unlikely for our sample to be filled with really rare data-points if we used random-sampling (which we must have used if we assume data points are $\text{i.i.d.}$)
- Idea -> choose $\theta$ such that we maximize
	- $$\huge p(D) = \prod_{i} p(x_{i}) p_{\theta}(y_{i} | x_{i})$$
	- Problem
		- $p(x_{i})$ -> Multiplying together many numbers $\leq 1$
		- We end up getting a number really close to $0$
- $$\huge \begin{align}\log p(D) &= \sum_{i} \log p(x_{i}) + \log p_{\theta}(y_{i} | x_{i}) \\ &= \sum_{i} \log p_{\theta}(y_{i} | x_{i}) + \text{const} \\ &= \sum_{i} \log p_{\theta}(y_{i} | x_{i})\end{align}$$
	- Take logarithm of both sides of $p(D)$ equation
		- Choosing $\theta$ to maximize $\log p(D) =$ Choosing $\theta$ to maximize $p(D)$
			- $\log(x)$ is monotonically increasing, and maps $(-\infty, \infty)$ to $[0, \infty)$ 
				- Therefore any drops in $x$ will lead to a drop in $\log(x)$
				- The sign of the change between points is preserved, although the scale of the change is different
			- ![[Pasted image 20250207010206.png]]
				- Notice how even though the maximum itself changes, the $x$ position of the maximum is still $x = 10$
	- Log rules -> $\log_{a}bc = \log_{a}b + \log_{a}c$
		- Product inside of logarithms can be simplifies to a summation
	- $\sum_{i} \log p(x_{i})$ is a constant, because it does not depend on $\theta$
		- We are interested in finding a $\theta$ that maximizes $p(D)$
- Maximum likelihood estimation (MLE)
	- $$\huge \theta^* \leftarrow \text{arg max}_{\theta} \log p(D) = \text{arg max}_{\theta} \sum_{i} \log p_{\theta}(y_{i} | x_{i})$$
	- $\log p_{\theta}(y_{i} | x_{i})$ = likelihood
	- We want to choose $\theta$ to maximize this likelihood
- Negative log-likelihood (NLL)
	- $$\huge \theta^* \leftarrow \text{arg min}_{\theta} -\sum_{i} \log p_{\theta}(y_{i} | x_{i})$$
	- A version of MLE that has a negative number
	- Optimization literature canonically formulates optimization problems as minimization problems
	- This is our loss function!
# Loss Function
- In general
	- Loss function quantifies how bad $\theta$ is
	- We want the least bad $\theta$
		- This is the best $\theta$
- Examples
	- Negative log-likelihood -> $\sum_{i} \log p_{\theta}(y_{i} | x_{i})$
		- Also called "cross-entropy"
		- **DEFINE:** Cross-entropy
			- How similar two distributions $p_{\theta}$ and $p$ are
			- $$\huge H(p, p_{\theta}) = - \sum_{y} p(y | x_{i}) \log p_{\theta}(y | x_{i})$$
		- Assume $y_{i} \sim p(y | x_{i})$
		- $H(p, p) \approx - \log p_{\theta}(y_{i} | x_{i})$
			- $\sum_{y} p(y)$
	- Zero-one loss
		- $$\huge\sum_{i} \delta (f_{\theta}(x_{i} \neq y_{i}))$$
		- $$\huge \delta(x_{i}, y_{i}) \begin{cases}0 &\text{if } x_{i} = y_{i} \\ 1 &\text{if } x_{i} \neq y_{i}\end{cases}$$
			- Loss of 0 -> right answer
			- Loss of 1 -> wrong answer
	- Mean squared error
		- Useful if you're predicting a number instead of a label
		- $$\huge \sum_{i} \frac{1}{2} || f_{\theta}(x_{i}) - y_{i}||^2$$
		- Actually just negative log-likelihood