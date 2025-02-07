# Terminology and Notation
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
# Imitation Learning
![[Pasted image 20250206165327.png]]
- Goal -> learn policies with supervised learning algorithms
- Example
	- Autonomous driving
	- Observation -> images from dashboard
	- Actions -> steering
- Can collect data (observations and actions) from humans driving and create data set
- Can then train neural network to predict $a_{t}$ given $o_{t}$ 
- Algorithm is referred to behavior cloning
# Original Deep Imitation Learning System
- ALVINN: Autonomous Land Vehicle in a Neural Network
	- 1989
	- ![[Pasted image 20250206165413.png]]
# Does it Work? -> No!
![[Pasted image 20250206165629.png]]
- The policy $\pi_{\theta}$ may make small mistakes that puts agent in a unfamiliar state
	- The more unfamiliar the new state, the more mistakes it may make and the agent continues to deviate from the training trajectory
- Supervised learning doesn't suffer from this, because it assumes the training data follows the iid. property
	- iid -> independent and individually distributed
	- This means training data is not related to each other
	- However, in imitation learning, the previous state is related to the current state!
# Does it Work? -> Yes!
![[Pasted image 20250206170008.png]]
- Nvidia made an updated version of ALVINN, but required a large data set
## Why did that work?
- 