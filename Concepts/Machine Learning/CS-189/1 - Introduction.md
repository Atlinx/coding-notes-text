# 1 Introduction
# What is machine learning?
- Machine three core components -> model, optimization, data
- Model -> function of inputs to outputs
	- Not programmed by hand 
- Optimization algorithm finds good parameters
	- Parameters are "good" if it fits the data well
- Data typically consists of input-output examples
- Goal -> let the model generalize to data that it hasn't seen yet
# Types of machine learning problems
- Regression
	- ![[Pasted image 20250328080241.png]]
	- Output = real values
	- Guess target from inputs
- Classification
	- Output -> one of k values (classes)
	- **Ex.** Binary classification -> learn model that predicts whether input is green O or red X
		- ![[Pasted image 20250328080251.png]]
# Model for classification
- Model is a parameterized function
	- ![[Pasted image 20250328080404.png]]
		- Linear classifier -> model is a line that splits data into two halves
	- Trying to predict from $x_{1}, x_{2}$ whether the data point is a green O or a red X
	- Line
		- $$\theta_{1} x_{1} + \theta_{2} x_{2} + \theta_{3} \leq 0$$
		- $\leq 0$ means the data is below the line
		- $\geq 0$ means the data is above the line
- Parameterized functions are denoted as
	- $f(x, \theta) = y$
	- $f_{\theta}(x) = y$
	- $\theta$ are there parameters
# Examples of machine learning problems
- Recognizing digits -> classification
- Determining whether an email is spam -> classification (binary)
- Predicting price of a stock six months from now -> regression
- Predicting rating of a movie by a user -> either
- Determine credit worthiness for a mortgage or a credit card transaction -> classification
	- **ASIDE:** Before applying ML, consider the impact of bias, because ML reflects the biases of the data it's been trained on
# A classification example
# A linear classifier
- ![[Pasted image 20250328081153.png]]
- Linear classifier in the example above fits the data okay
- Linear classifier is weak in decision boundaries it can represent
# "Nearest neighbors" classifier
- "I will judge you by the company you keep"
- ![[Pasted image 20250328081545.png]]
- 1 nearest neighbor
	- Look at the closest example I was given, and use that as the label
	- Can identify islands of points
- 15 nearest neighbors
	- Look at closest 15 examples, and take a majority vote
	- Has a "smoothing" effect, outliers don't affect prediction as much anymore
# The "Bayes optimal" classifier
![[Pasted image 20250328081810.png]]
- Optimal classifier is determined by the distribution that generates the data
	- We can compute the optimal classifier if we know the distributions used to create the data
	- Will incur lower loss than any other possible classifier you can define
- In real life, you don't know the true distribution of the data, and are only given examples
	- Therefore you cannot calculate the optimal classifier
# Another classification example
- Digit recognition on the MNIST dataset
- ![[Pasted image 20250328082043.png]]
- U.S. postal office created this data set of scanned hand-written digits
	- Wanted to train machine learning model that can automatically read the digit on the 
- Has training set of 60,000 examples, and test set of 10,000 examples
	- Use training set to train model
	- Use test set to test the model
		- Want to test the model's ability to generalize on data it hasn't seen
		- What if model just memorizes the training data?
- Each digit is 28 x 28 pixel grayscale image
- Test error tests of best systems are under 0.5%
	- Likely better than human performance
# Linear model for digit classification?
![[Pasted image 20250328082449.png]]
- Images 28 x 28 = 784 dimensional
- This is really tiny image
	- Larger image could be 224 x 224 x 3 (RGB) = 150528 dimensional
- More higher dimensional a problem becomes, less likely a linear model would work well
	- Different dimensions between input may not have linear relationships between each other
# Training set vs. test set vs. validation set error
- Training set error
	- What we train classifier to minimize
- Test set error
	- What we actually care about
	- How well does the classifier generalize to new data?
	- If the test set error $>$ training set error, then the classifier has **overfit** to the training set
- Validation set
	- Set of data part of the training set that's not used for training and used to evaluate the model
	- Used to combat overfitting
- Validation sets can be used to decide when to stop training and to choose **hyperparameters**
- Hyperparameters
	- Parameters that you choose for the model
	- **Ex.** Number of neighbors to use
# Bias vs. variance of learning algorithms
![[Pasted image 20250328083125.png]]
- Bias
	- How far off from the correct answer you are
- Variance
	- What is the spread of your answers?
	- **Ex.** Training model with different samples of the training data leading to multiple
- Want to minimize both bias and variance
![[Pasted image 20250328083332.png]]
- Bias variance tradeoff (classical idea)
	- Model complexity
		- Linear classifier -> low complexity
- In practice
	- We can train huge neural networks, which don't have a lot of variance
- As we increase the model's complexity
	- Bias decreases -> model is able to better fit to the data and output the correct answer
	- Variance increases -> we need more and more data to train it
	- There should be a sweet-spot in the middle where we trade-off between bias and variance
# Closing Question
- Classical view of machine learning
	- Predict y from x
- What is $x$?
- 