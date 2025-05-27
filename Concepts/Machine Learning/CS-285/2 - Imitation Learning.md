# 2 Imitation Learning
# Supervised learning of behaviors
## Terminology and Notation
![[Pasted image 20250206163028.png]]
- Observation -> $o_{t}$
	- $$\huge o_{t}$$
	- Observation at a given point in time ($t$)
	- Incomplete view of the state
		- Might not be possible to infer state from observation
	- **Ex.**
		- $t = 0 \to$ 0 ms from start
		- $t = 200 \to$ 200 ms from start
	- **Ex.** 
		- $o_{t} =$ image of tiger
- Policy -> $\pi(a_{t} | o_{t})$
	- $$\huge \pi_{\theta}(a_{t}|o_{t})$$
	- Policy are distributions that assign probability for all actions given an observation
	- Stochastic policies -> policies that assign a probability for all actions given an observation
		- Training policies as probability distributions is the most convenient
			- Can still make a decision in the end by taking the action with the highest probability assigned to it
	- Deterministic policies
		- Special case of stochastic policies
		- Assigns probability of 1 for the action that it will take
	- At time $t$, Given an observation $o_{t}$ we choose an action $a_{t}$
	- Partial vs. full observation
		- Partial observation policy -> $\pi_{\theta}(a_{t} | s_{t})$
			- The action we take is dependent on our observation of the world (which might be incomplete)
		- Full observation policy -> $\pi_{\theta}(a_{t}|s_{t})$
			- The action we take is dependent on the state of the world
- State -> $s_{t}$
	- Concise and complete explanation of the world, including what you might not be able to see
	- What observations originate from
		- Can always figure out an observation from the current state
- Action -> $a_{t}$
	- $$\huge a_{t}$$
	- Action you take based 
	- Action space
		- Discrete 
			- **Ex.** Run away, ignore, pet
		- Continuous
			- Output would be parameters of continuous distribution
- ![[Pasted image 20250206164621.png]]
	- **DEFINE:** Markov property
		- States are conditionally independent
			- If you know the full state, you can make future predictions
			- You don't need to know anything about the past states
	- $p(s_{t+1}|s_{t}, a_{t})$
		- $p$ is a transition function
		- Transitions from one state $s_{t}$ to another $s_{t+1}$ given an action $a_{t}$
- **NOTE:**
	- Some reinforcement learning algorithms require Markov property (know the entire state $s$)
	- Some reinforcement learning algorithms can work with just observations $o$
## Aside: Notation
- Notation developed by Richard Bellman as part of DP
	- $s_{t}$ -> state
	- $a_{t}$ -> action
- Used in controls
	- Developed Lev Pontryagin, who was Russian
	- $x_{t}$ -> state
	- $u_{t}$ -> action
## Imitation Learning
![[Pasted image 20250206165327.png]]
- Goal -> learn policies with supervised learning algorithms
- Example
	- Autonomous driving
	- Observation -> images from dashboard
	- Actions -> steering
- Can collect data (observations and actions) from humans driving and create data set
- Can then train neural network to predict $a_{t}$ given $o_{t}$ 
- Algorithm is referred to behavior cloning
## Original Deep Imitation Learning System
- ALVINN: Autonomous Land Vehicle in a Neural Network
	- 1989
	- ![[Pasted image 20250206165413.png]]
## Does it Work? -> No!
![[Pasted image 20250206165629.png]]
- The policy $\pi_{\theta}$ may make small mistakes that puts agent in a unfamiliar state
	- The more unfamiliar the new state, the more mistakes it may make and the agent continues to deviate from the training trajectory
- Supervised learning doesn't suffer from this, because it assumes the training data follows the iid. property
	- iid -> independent and individually distributed
	- This means training data is not related to each other
	- However, in imitation learning, the previous state is related to the current state!
## Does it Work? -> Yes!
![[Pasted image 20250206170008.png]]
- Nvidia made an updated version of ALVINN, but required a large data set
## Why did that work?
- ![[Pasted image 20250408160201.png]]
- Car has both left and right cameras that provide additional data for each driving session
	- If the car sees an image similar to the one on the left, then we label the data to move on the right
	- Therefore we give the car states that are similar to what it would see if it makes a mistake
		- Rather than just giving it expert trajectories
## Moral of story, and a list of ideas
- Imitation learning via behavioral cloning is not guaranteed to work
	- This is different from supervised learning
	- Reason -> i.i.d. assumption does not hold!
- We can formalize why this is and do a bit of theory
- Address problem in a few ways
	- Be smart about how we collect (and augment) our data
		- **Ex.** NVidia adding synthetic data to address states mistakes
	- Use very powerful models that make very few mistakes
	- Use multi-task learning
		- Learning multiple tasks at the same time
			- ![[Pasted image 20250408160458.png]]
	- Change the algorithm (DAgger)
# Why does behavioral cloning fail?
## The distributional shift problem
![[Pasted image 20250408160606.png]]
- $\pi_{\theta}(a_{t} \mid o_{t})$
	- Policy that makes an action based on an observation
- $p_{\text{data}}(o_{t})$
	- Distribution that produced the data set
	- We train our policy under this distribution
- $p_{\pi_{\theta}}(o_{t})$
	- Distribution of observations that the actual model sees
- $$\huge \underset{ \theta }{ \max } \mathbb{E}_{o_{t} \sim p_{\text{data}}(o_{t})}[\log \pi_{\theta}(a_{t} \mid o_{t})]$$
	- When we trained our policy $\pi_{\theta}$, we want to maximize the likelihood of taking the correct action $a_{t}$ when given an observation $o_{t}$ for all possible observation and action pairs in our training dataset
	- We test under $p_{\pi_{\theta}}(o_{t})$
	- $p_{\text{data}}(o_{t}) \neq p_{\pi_{\theta}}(o_{t})$
		- This represents a **distributional shift**
		- The distribution we test under is different from the distribution that we've trained on
## Let's define more precisely what we want
- ![[Pasted image 20250408161418.png]]
- What makes learned $p_{\theta}(a_{t \mid o_{t}})$ good or bad?
	- $$\huge \underset{ \theta }{ \max } \mathbb{E}_{o_{t} \sim p_{\text{data}}(o_{t})}[\log \pi_{\theta}(a_{t} \mid o_{t})]$$
	- Not god enough,
- Instead, use
	- $$\huge c(s_{t}, a_{t}) = \begin{cases}
		0 \text{ if } a_{t} = \pi^*(s_{t}) \\
		1 \text{ otherwise}
		\end{cases}$$
	- $c$ = cost
		- Cost is $0$ if the action is the same as the human action
			- Assume the human driver has a deterministic policy $\pi^*$
		- Cost is $1$ otherwise
		- Let $\epsilon =$ probability of making a mistake
			- Since the cost is only 1 if we make an error, its expected value is $\epsilon$
	- All the analyses following this will use $s$ (states) instead of $o$ (observations)
		- More difficult to perform analysis in partially observed setting
	- Goal
		- Minimize $$\huge \mathbb{E}_{s_{t} \sim p_{\pi_{\theta}}(s_{t})}[c(s_{t}, a_{t})]$$
		- Minimize the number of mistakes while under the ACTUAL POLICY $\pi_{\theta}$ rather than on the training dataset
## Some analysis
![[Pasted image 20250408163404.png]]
- Assume $s \in \mathcal{D}_{\text{train}}$
	- This means our states are all from the distribution of states in the training data
- Assume $\pi_{\theta}(a \neq \pi^*(s) \mid s) \leq \epsilon$
	- This means the probability of choosing an action that's not the expert action in a given state is $\leq \epsilon$
	- Therefore the probability of us making a mistake is $\leq \epsilon$
	- Therefore for our next analysis, we know
		- $1 - \epsilon =$ probability of not making a mistake
		- $\epsilon =$ probability of making a mistake
- Let $T =$ total number of time steps
- We want to write an upper-bound (worst-case bound) for the total cost
	- This bound would essentially be making as many mistakes as possible
	- $$\Large \underbrace{ \mathbb{E} \left[ \sum_{t} c(s_{t},a_{t}) \right] }_{ O(\epsilon T^2) } \leq \underbrace{ \epsilon T + (1 - \epsilon)(\epsilon (T - 1) + (1 - \epsilon)(\epsilon(T - 2) + \dots)) }_{ T \text{ terms, each } O(\epsilon T) }$$
		- Therefore we include the case of
			- Making a mistake in all timesteps $T$
			- Succeeding until step $1$, but making mistakes in remaining timesteps $T-1$
			- Succeeding until step $2$, but making mistakes in remaining timesteps $T-2$
			- ...
		- We have $T$ terms
			- **NOTE:** $(1 - \epsilon) \approx 1$ , because $\epsilon$ is so small
			- Therefore order of each term is $O(\epsilon T)$
	- Therefore in the worst case, number of mistakes is $O(\epsilon T^2)$ for small values of $\epsilon$
## More general bound
- ![[Pasted image 20250408171417.png]]
- Assume states are sampled from the training set
	- For states sampled from the training set, our policy makes an error at a rate of $\leq \epsilon$ 
- Assume expected value of loss $\leq \epsilon$
- If $p_{\text{train}}(s) \neq p_{\theta}(s)$
	- $$\huge p_{\theta}(s_{t}) = (1 - \epsilon)^t p_{\text{train}}(s_{t}) + (1 - (1 - \epsilon)^t) p_{\text{mistake}}(s_{t})$$
	- We have two terms
		- 1 -> Probability of making no mistakes
		- $p_{\text{mistake}}(s_{t}) =$ Some other distribution
	- Absolute value means total variation divergence
		- Sum of $|p_{\theta}(s_{t}) - p_{\text{train}}(s_{t})|$ for all states $s_{t}$
			- You can think of in the "biggest case,"
				- There are just two states $s_{1}$, $s_{2}$
				- We know that the sum of the probabilities of each state must be $1$
					- $p_{\theta}(s_{1}) + p_{\theta}(s_{2}) = 1$
					- $p_{\text{train}}(s_{1}) + p_{\text{train}}(s_{2}) = 1$
				- However, $p_{\theta}(s_{1}) = 1$, $p_{\theta}(s_{2}) = 0$, and $p_{\text{train}}(s_{1}) = 0$, $p_{\text{train}}(s_{2}) = 1$
					- Therefore, when we calculate variation divergence of each possible state, we get
						- $|p_{\theta}(s_{1}) - p_{\text{train}}(s_{1})| = |1 - 0| = 1$
						- $|p_{\theta}(s_{2}) - p_{\text{train}}(s_{2})| = |0 - 1| = 1$
					- Therefore, the sum of all possible variation divergences = $1 + 1 = 2$
- We can then calculate the expected value of the costs over $p_{\theta}(s_{t})$, the distribution of states according to our trained policy $\pi_{\theta}$
	- This expected value represents the total cost we expect to see on average
	- We want to find a worst case bound for this, which is why we can use $\leq$ in the steps following this equation
## Why is this rather pessimistic?
- ![[Pasted image 20250408171928.png]]
- In reality, we can often recover from mistakes
- This doesn't mean that imitation learning will allow us to learn how to do that!
- ![[Pasted image 20250408172000.png]]
- Paradox
	- Imitation learning works better if the data has more mistakes (and recoveries)!
# Addressing the problem in practice
# What 
- Intentionally add mistakes  and correction