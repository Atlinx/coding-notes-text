# 14 - Imitation Learning
## So far: learning to predict
- Predict category of image
- Captioning text
- Predicting words based on sounds
- What about learning to control?
	- ![[Pasted image 20250405083024.png]]
	- Self driving cars
## From *prediction* to *control*: challenges
- Prediction
	- $\text{i.i.d.}: p(\mathcal{D}) = \prod_{i} p(y_{i} \mid x_{i}) p(x_{i})$
		- Output $y_{1}$ does not change $x_{2}$
	- Focus on just getting highest average accuracy over the whole dataset
	- Concrete goals
		- Ground truth labels
- Control
	- ![[Pasted image 20250405083156.png]]
	- Choice made in previous positions affect future choices
		- Making the wrong choice is perhaps OK
		- Making wrong choice here is disaster
	- Abstract goals
		- "Drive to grocery store"
		- What steering command is that?
### Summary
- Prediction
	- i.i.d. distributed data (each datapoint is independent)
	- Ground truth supervision
	- Objective is to predict the right label
- Control
	- Each decision can change future inputs
	- Supervision may be high-level
	- Objective is to accomplish the task
- We will build up toward a reinforcement learning system that addresses all of these issues, but we'll do so one piece at a time
- Issues above are not just issues for control, in many cases real-world deployment of ML has same feedback issues
	- **Ex.** Decisions made by traffic prediction system might affect whole route that people take, which changes traffic
## Terminology
- $o$
	- Observations
	- This is our input
	- Used to be $x$
- $\pi_{\theta}(a_{t} \mid o_{t})$
	- Used to be $p_{\theta}(y \mid x)$
- $a$
	- Action
	- What we predict
	- Used to be $y$
- $s_{t} =$ state
	- The real physical system, the underlying variables, that describe the world
	- **Ex.** position + velocity for the cheetah 
	- ![[Pasted image 20250405123121.png]]
- $o_{t} =$ observation
	- What we see in the world
- $a_{t} =$ action
- Observation + state
	- ![[Pasted image 20250405123213.png]]
	- Bayesian network of control
	- State satisfies **markov property**
		- State $S_{3}$ is conditionally independent from $S_{1}$ and $S_{2}$
		- A state summarizes everything you need to know to predict what happens in the future
	- Observations **do not obey the markov property**
		- Knowing what happened in the past may help in predicting the future
			- **Ex.** Car driving in front of cheetah, knowing the cheetah existed before means we can infer the cheetah still exists
## Aside: notation
- Richard Bellman -> Reinforcement learning/dynamic programming literature
	- $s_{t} =$ state
	- $a_{t} =$ action
- Lev Pontryagin -> Controls literature
	- $x_{t} =$ states
	- $u_{t} =$ action
## Imitation Learning
- ![[Pasted image 20250405123726.png]]
- Dataset consists of observations from camera and driving commands from a human driver
	- Train the conv net to take in an image and output an action 
- **Behavioral cloning**
## Does it work in theory? -> No
- ![[Pasted image 20250405124120.png]]
- The trained policy $\pi_{\theta}$ may deviate from the trained data
	- Now the model is in a different state that wasn't in its training data, and it's more likely to make mistakes
	- These mistakes compound, and eventually it gets far off track
## Does it work in practice? -> Yes
- NVIDIA trained model to drive car with 3000 miles of data
- Works decently well
## What is the Problem?
![[Pasted image 20250405124407.png]]
- Policy
	- $\huge \pi_{\theta}(a_{t} \mid o_{t})$
- $\huge p_{\text{data}(o_{t})} \neq p_{\pi_{\theta}}(o_{t})$
- This is similar to what we saw in [[10 - Recurrent Neural Networks#Aside distributional shift|distribution shift with RNNs]]:
	- ![[Pasted image 20250405124733.png]]
	- This is training/test discrepancy
	- The network always saw true sequences as inputs, but as test time it gets as input its own (potentially incorrect) predictions
	- This is called **distributional shift** because input distribution shifts from true strings to synthetic strings
## Why not use the same solution?
- The problem: $p_{\text{data}}(o_{t}) \neq p_{\pi_{\theta}}(o_{t})$
- **Before**: scheduled sampling
	- Can we use [[10 - Recurrent Neural Networks#Aside scheduled sampling|scheduled sampling]] from RNNs?
	- ![[Pasted image 20250405125108.png]]
- **Now:** control
	- We could take predicted action $a_{t} \sim \pi_{\theta}(a_{t} \mid o_{t})$ and observe the resulting $o_{t+1}$
	- However this requires interacting with the world!
		- We are only working off of recordings of driving at the moment
		- Why?
	- We don't know $p(s_{t+1}|s_{t}, a_{t})$
		- ![[Pasted image 20250405125011.png]]
		- We can only figure out $s_{t}$
		- There are some algorithms that attempt to learn this probability (model-based reinforcement learning)
## Can we **mitigate** the problem?
- The problem: $p_{\text{data}}(o_{t}) \neq p_{\pi_{\theta}}(o_{t})$
- If $\pi_{\theta}(a_{t}\mid o_{t})$ is very accurate
	- Maybe $p_{\text{data}}(o_{t}) \approx p_{\theta}(o_{t})$
	- Works if data is very broad (covers a huge range of observations)
- Why might we fail to fit the expert?
	1. Non-Markovian behavior
		- Learning policy $\pi_{\theta}(a_{t} \mid o_{t})$
		- Assumes behavior depends only on current observation
		- If we see the same thing twice, we do the same thing twice, regardless of what happened before
			- Often very unnatural for human demonstrators
			- Human behavior/actions may depend on everything that has happened so far
	2. Multimodal behavior
## How can we use the whole history?
- We can use an RNN with a convolutional encoder
	- ![[Pasted image 20250405125547.png]]
	- More specifically, an LSTM
- Using RNN to model non-markovian
## Multimodal behavior
- Drone that is flying towards a tree
	- Can fly to the left or right
		- ![[Pasted image 20250405125820.png]]
	- We have an action that is continuous, the output would be a distribution
		- If we averaged this distribution, we'd end up going neither left nor right
	- If we discretized the actions (forcing them to be a specific action like "left" or "right")
		- ![[Pasted image 20250405125906.png]]
		- However this is not really feasible for high dimension actions
			- Number of discrete things needed to represent all actions grows exponentially
	- Solutions
		1. Output mixture of gaussians
		2. Latent variable models (discussed later)
		3. Autoregressive discretization
## Mixture of Gaussians
- Output the mean and variance for each of $N$ possible gaussian mixture models
	- ![[Pasted image 20250405130239.png]]
	- If you see a tree, you can have two mixture models -> one for fly left, and one for fly right
	- We can have the neural network itself output the $\mu$ and $\sigma$ of the mixture models
	- Drawbacks
		- In high dimension spaces, it's very inefficient
		- Might need exponentially large # of distributions
## Latent variable models
- Reason why you drive left or right may be dependent on hidden variables, like your feelings at that moment, etc.
	- ![[Pasted image 20250405130610.png]]
- We can have neural network take in image and additional vector that represents unknown variable $\mathcal{E}$
	- At test time, we sample this variable randomly
	- At training time, we have to also guess these variables
		- Look up
			- Conditional variational autoencoder
			- Normalizing flow/realNVP
			- Stein variational gradient descent
## Autoregressive discretization
- Discretizing action works well if we have low dimensions
- Autoregressive discretization -> Discretize one dimension at a time
	- ![[Pasted image 20250405130850.png]]
	- We sample a single dimension in the action, and then feed that into another network, to get the second action
		- Can implement this using a [[10 - Recurrent Neural Networks#RNN encoders and decoders|conditional RNN decoder]]
	- Downsides
		- Sampling is easy
		- Finding the max likely action require beam search
## Why did that work?
- ![[Pasted image 20250405131135.png]]
- Images from left camera is labelled with the driving command + a slight steering to the right
	- ![[Pasted image 20250405131322.png]]
- Images from right camera is labelled with the driving command + a slight steering to the left
	- ![[Pasted image 20250405131331.png]]
- Images from front camera is just labelled with driving command
	- ![[Pasted image 20250405131518.png]]
- The model only uses the single front camera when it's actually running
	- We're effectively generating 3 x datapoints for the price of 1 test by using 3 cameras
	- If the car starts to veer left, the main camera will see images similar to the left camera, and therefore the model will use what is learned and begin moving right
	- If the car starts to veer right, the main camera will see images similar to the right camera, and therefore the model will use what it learned and begin moving left
- Effect of distribution shift is mitigated, because it provides the model with examples that might look like what happens at test time
	- It pushes the model to compensate for left and right turns
## Summary
- In principle, it should not work
	- Distribution mismatch problem
- Sometimes works well
	- Hacks (left/right images)
		- Application dependent
	- Models with memory (**Ex.** RNNs)
	- Better distributional modeling
	- Generally taking care to get accuracy
## Can we make it work more often?
- Can we make $p_{\text{data}}(o_{t}) = p_{\pi_{\theta}}(o_{t})$?
	- Idea -> instead of being clever about $p_{\pi_{\theta}}(o_{t})$ be clever about $p_{\text{data}}(o_{t})$
- **DAgger** -> Dataset Aggregation
	- Goal -> collect training data from $p_{\pi_{0}}(o_{t})$ instead of $p_{\text{data}}(o_{t})$
	- How? just run $\pi_{\theta}(a_{t} \mid o_{t})$
	- But need labels $a_{t}$
		1. Train $\pi_{\theta}(a_{t} \mid o_{t})$ from human data $\mathcal{D} = \{ o_{1}, a_{1}, \dots, o_{N}, a_{N} \}$
		2. Run $\pi_{\theta}(a_{t} \mid o_{t})$ to get dataset $\mathcal{D}_{\pi} = \{ o_{1}, \dots, o_{M} \}$
		3. Ask human to label $D_{\pi}$ with actions $a_{t}$
		4. Aggregate $\mathcal{D} \leftarrow \mathcal{D} \cup \mathcal{D}_{\pi}$
	- This works because eventually the $p_{\text{data}}(o_{t}) \approx p_{\pi_{\theta}}(o_{t})$
- Example
	- Dagger trained to fly drone through trees
- Drawbacks
	- Unsafe to let the model run the system
	- Humans are not good at making decisions when the environment isn't 100% accurate
		- Humans might drive differently in front of a virtual steering wheel that doesn't react to what they do
	- Data collection protocols is hard
## Summary and takeaways
- In principle it should not work
	- Distribution mismatch problem
	- DAgger can address this, but requires costly data collection and labelling
- Sometimes works well
	- Requires a bit of (heuristic) hacks, and very good (high-accuracy) models
- Recommendation -> try behavioral cloning first, but prepare to be disappointed