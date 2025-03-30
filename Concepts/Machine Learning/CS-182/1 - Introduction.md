# 1 Introduction
# Translating Languages
- Instead of translating languages directly into one-another, researchers tried building a machine that translates languages into an "intermediate language"
	- This represents the "thought" of humans, which can then be translated into any other language
	- Found this to be more effective than direct translation
# Representation Learning
- Classic view of machine learning
	- Predict $y$ from $x$
- What is $x$?
	- Language
	- Decision-making
- Handling complex inputs requires representations
	- **Ex.** Handling "thoughts" when translating between languages
- Power of deep learning lies in ability to learn representations automatically from data
# What Is Machine Learning?
- Example
	- ![[Pasted image 20250206171113.png]]
	- Write a program to take in photos and identifies what's in the photo -- whether there is a puppy or not
- How do we implement this program?
	- Function is a set of rules for transforming inputs into outputs
		- Programs are functions
	- Sometimes we can define rules by hand -> programming
	- What if we don't know the rules?
	- What if rules are too complex?
		- We'd need to write a program with a huge number of if statements
- Instead of defining input -> output relationship by hand, define a program that acquires this relationship from **data** or **examples** than to implement those rules

> [!info] Key Idea
> - If rules that describe how **inputs** map to **outputs** are complex and full of special cases and exceptions, it is easier to provide data or examples than to implement those rules
> - These problems are great for machine learning
- Question -> Does this apply to human and animal learning?
	- Humans sometimes prefer to learn from examples and from practice
# What Are We Learning?
- Example
	- ![[Pasted image 20250206172507.png]]
	- Machine is learning where to place a line that divides circles from crosses
	- Equation that describes a line -> $x_{1} \theta + x_{2} \theta_{2} + \theta_{3} \leq 0$
		- For a given input $x$, if `below_line(x)` then -> output circle
			- else -> output cross
		- $\theta$ are the parameters we want to learn
			- We want a set of parameters that can create a line that accurately divides circles from crosses 
		- Parameters can be represented as a vector
			- $\text{learn}(\theta_{1}, \theta_{2}, \theta_{3}) = \vec{\theta}$
			- We can therefore represent the equation using vectors

$$\huge 
\begin{align}
\vec{x}^T\vec{\theta} \leq 0 \\ \\
\begin{bmatrix}
x_{1} & x_{2} & x_{3}
\end{bmatrix} \times \begin{bmatrix}
\theta_{1} \\
\theta_{2} \\
\theta_{3}
\end{bmatrix} \leq 0
\end{align}$$

- We want to learn the parameters $\theta$ such that our parameterized program (function) gives the right answer!
# In General
$$
\begin{align}
\huge f(x, \theta) = y
\end{align}
$$
- $x =$ inputs
- $\theta$ = parameters -> these are learned
- $y$ = output
	- **Ex.** label
- We can also write it as
$$\huge f_{\theta}(x) = y$$
- Crucially, $f_{\theta}(x)$ can be almost any expression of $x$ and $\theta$!
# But What Parameterization Do We Use?
- ![[Pasted image 20250206173019.png]]
- We identify features of puppies, like their ears, eyes, etc.
- When computer looks at pictures, it just sees numbers
# "Shallow" Learning
- ![[Pasted image 20250206173351.png]]
- "Feature-based" learning
- $f_{\theta}(x) = y$
	- Rather than processing the input $x$ outright, let's try to simplify it first
	- $\phi(x)^T \theta \leq 0$
		- Let $\phi(x)$ be a fixed function that extracts features from $x$
			- Features -> extracting edges, corners from image, etc.
			- Features can potentially simply $x$ and make it easier to use -> maybe instead of inputting an 100 x 100 image, we can extract 10 features
- ![[Pasted image 20250206173317.png]]
	- **Ex.** Histogram of oriented gradients
		- In little patch of image, extract edges
			- Then construct histogram of orientation of the edges in the patch
		- Then construct a new "low-res" image where each box contains a histogram of edges
- Using features
	- "Compromise" solution
		- Don't hand-program the rules, but hand-program the features
	- Learning on top of the features can be simple
	- Coming up with good features is very hard
# From Shallow Learning to Deep Learning
![[Pasted image 20250206174408.png]]
- What if we learn the parameters to the features?
	- Features become learning features
# Multiple Layers of Representations
![[Pasted image 20250206174730.png]]
- Each arrow represents a simple parameterized transformation (function) of the preceding layer
- The actual representation used by deep learning
	- Lowest level features look like edges
	- Higher level features look like parts
- Higher level representations
	- More abstract
	- More invariant to nuisances
		- **Ex.** If the puppy in the input image is looking in different direction
	- Easier for predicting label
# What is Deep Leering?
- Machine learning with multiple layers of learned representations
- The function that represents the transformation from input to internal representation to output is usually a deep neural network
	- Almost all **multi-layer parametric functions** with learned parameters can be called neural networks
- Parameters for every layer are usually trained with respect to overall task objective (**Ex.** accuracy in detecting puppies)
	- Sometimes referred to as end-to-end learning
- **DEFINE:** End-to-end learning
	- Training every layer with respect to overall task objective
# What Makes Deep Learning Work?
- History
	- 1950 -> Turing first describes how learning could lead to machine intelligence
	- 1957 -> Rosenblatt's perceptron proposed as practical learning method
		- Algorithm that creates a linear separation of classes
	- 1969 -> Minsky & Papert publish book describing limitations of neural networks
		- **Ex.** Difficult to find global optimum
		- After publication, mainstream research focused on "shallow" learning
	- 1986 -> Backpropagation as practical method for training deep nets
	- 1989 -> LeNet, Neural network for handwriting recognition
		- Huge wave of interest in ML community in probabilistic methods, but still shallow models
	- 2006 -> Deep neural networks gain more attention
	- 2012 -> Krizhevsky's AlexNet paper beats all methods on ImageNet (image recognition benchmark)
- What makes it work?
	1. **Big** models with many layers
		- Better captures complex representations
	2. **Large** datasets with many examples
		- Humans also learn from a large amount of data
	3. Enough **compute** to handle all this
		- Can't wait for years to years for training
# Model Scale: Is More Layers Better?
- LeNet -> 7 layers (1989)
	- ![[Pasted image 20250206183550.png]]
- Krizhevsky's model (AlexNet) for ImageNet -> 8 layers (2012)
	- Much larger layers
	- ![[Pasted image 20250206183641.png]]
- ResNet-152 -> 152 layers (2015)
	- ![[Pasted image 20250206183726.png]]
	- We can see the more layers, the better the performance (lower errors)
# How Big Are The Datasets?
- MNIST (handwritten characters), 1990s - today -> 60k images
- CalTech 101, 2003 -> 9k images
- CIFAR 10, 2009 -> 60k images
- ILSVRC (ImageNet), 2009 -> 1.5 million images
# How Does It Scale With Compute?
- ![[Pasted image 20250206184230.png]]
- What about NLP (Natural language processing)?
	- How long does it take to train BERT (widely used language model)?
		- 54 hours on about 16 TPUs
	-  TPU -> piece of custom hardware for training large neural networks
	- 16 TPU > 16 GPUs
# So.. It's Really Expensive?
- One perspective
	- Deep learning is not a good idea, because it requires huge models, huge amounts of data, and huge amounts of compute
- Another perspective
	- Deep learning is great -> as we add more data, more layers, and more compute, the models get better and better!
- ![[Pasted image 20250206184442.png]]
	- Neural networks are already outperforming humans
# Underlying Themes
- Acquire representations using high-capacity models and lots of data, without requiring manual engineering of features or representations
	- **DEFINE:** Model capacity
		- (informally) How many different functions a particular model class can represent
		- **Ex.** Linear decision boundaries vs. non-linear boundaries
	- Automation
		- We don't need to know what the good features are, can have model figure it out from data
	- Better performance
		- When representations are learned end-to-end, they are better tailored to the current task
- Learning vs. inductive bias ("nature vs. nurture")
	- Models get most performance from data rather than from designer insight
	- **DEFINE:** Inductive bias
		- What we build into the model to make it learn effectively
		- (informally) Built-in knowledge or biases in a model designed to help it learn
			- All knowledge is "bias" -> makes some solutions more likely and some less likely
	- Should we build in knowledge, **or** better machinery for learning and scale?
- Algorithms that scale
	- Often refers to methods that can get better as we add more data, representational capacity, and compute
	- **DEFINE:** Scaling
		- (informally) Ability for an algorithm to work better as more data and model capacity is added
# Why Do We Call Them Neural Nets?
- Early on, neural networks were proposed as rudimentary model of neurons in the brain
- Human neurons
	- ![[Pasted image 20250206185114.png]]
	- Dendrites receive signals from other neurons
	- Neuron "decides" whether to fire based on incoming signals
	- Axon transmits signal to downstream neurons
- Artificial neurons
	- ![[Pasted image 20250206185305.png]]
	- Artificial "neuron" sums up signals from upstream neurons $z = \sum_{i} a_{t}$
		- **NOTE:** Neurons are also referred to as units
	- Neuron "decides" how much to fire based on incoming signals
		- Activation function $a = \sigma(z)$
	- Activations transmitted to downstream units
- Is this a good model for real neurons?
	- Crudely models some neuron function
	- Missing many other important anatomical details
	- Don't take it too seriously
# What Does Deep Learning Have To Do With The Brain?
- Research found that features learned by neural networks are similar to features learned by animals 
- Does this mean the brain does deep learning?
- Or does it mean any sufficiently powerful learning machine will basically derive the same solution?
	- Similarity in problem itself
	- **Ex.** Airplanes adopt solutions similar to birds -> use wings
		- ![[Pasted image 20250206185656.png]]
	- This means deep learning cold be equally powerful
# Flashcards
#flashcards/machine-learning/cs-182/1-introduction

Machine learning
?
- We want a program that label an image
	- Machine learning is the idea of having the program acquire the relationship between images and labels from **data** or **examples** than to implement those rules
- It's a function that takes in some input $x$, and makes a prediction $y$
	- The function can be adjusted by tweaking the it's additional parameters $\theta$
- $$
\begin{align}
\huge f(x, \theta) = y
\end{align}
$$
	- $x =$ input
	- $\theta$ = parameters -> these are learned
	- $y$ = output
		- **Ex.** label

Shallow learning
?
- AKA "feature-based learning"
	- Traditionally, we trained shallow neural network
	- Learning directly from the input using these models is hard
- Rather than processing the input $x$ outright, let's try to simplify it first
- $\phi(x)^T \theta \leq 0$
	- Let $\phi(x)$ be a fixed function that extracts features from $x$
		- Features -> extracting edges, corners from image, etc.
		- Features can potentially simply $x$ and make it easier to use -> maybe instead of inputting an 100 x 100 image, we can extract 10 features
- ![[Pasted image 20250206173317.png]]

Shallow layer to deep learning
?
- What if we learned parameters to the features?
- Our model has many layers of features
	- ![[Pasted image 20250206174730.png]]
- Each arrow represents a simple parameterized transformation (function) of the preceding layer
- The actual representation used by deep learning
	- Lowest level features look like edges
	- Higher level features look like parts

End-to-end learning
?
- Training every layer with respect to overall task objective

We can see the more layers, the {{better}} the performance
?
- Lower errors

Deep learning
?
- Machine learning with multiple layers of learned representations
- The function that represents the transformation from input to internal representation to output is usually a deep neural network
	- Almost all **multi-layer parametric functions** with learned parameters can be called neural networks
- Parameters for every layer are usually trained with respect to overall task objective (**Ex.** accuracy in detecting puppies)
	- Sometimes referred to as end-to-end learning

What makes modern machine learning work?
?
1. **Big** models with many layers
	- Better captures complex representations
2. **Large** datasets with many examples
	- Humans also learn from a large amount of data
3. Enough **compute** to handle all this
	- Can't wait for years to years for training

Model capacity
?
- (informally) How many different functions a particular model class can represent
- **Ex.** Linear decision boundaries vs. non-linear boundaries

Underlying themes of deep learning
?
- Benefits of acquiring representations using high-capacity models and lots of data
- Learning vs. inductive bias
-  Algorithms that scale

Benefits of acquiring representations using high-capacity models and lots of data
?
- Automation
	- We don't need to know what the good features are, can have model figure it out from data
- Better performance
	- When representations are learned end-to-end, they are better tailored to the current task

Learning vs. inductive bias
?
- Models get most performance from data rather than from designer insight
- Should we build in knowledge, **or** better machinery for learning and scale?

Inductive bias
?
- What we build into the model to make it learn effectively
- (informally) Built-in knowledge or biases in a model designed to help it learn
	- All knowledge is "bias" -> makes some solutions more likely and some less likely

Scaling
?
- (informally) Ability for an algorithm to work better as more data and model capacity is added