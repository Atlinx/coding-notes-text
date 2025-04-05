# 15 - Policy Gradients
## From *prediction* to *control*
- From prediction to control
	- i.i.d distributed data (each datapoint is independent)
	- Ground truth supervision
	- Objective is to predict the right label
- Control
	- Each decision can change future inputs (not independent)
	- Supervision may be high-level (**ex.** a goal)
	- Objective is to accomplish a task
- These are not just issues for control -> many cases of real-world deployment of ML has these same feedback issues
	- **Ex.** Traffic prediction system that's deployed with users would then affect traffic patterns
## How do we specify what we want?
- ![[Pasted image 20250405133423.png]]
- Which action is better or worse?
- $r(s, a)$ -> reward function
	- Scores every state or every state + action pair
	- Tells us which states and actions are better
	- **Ex.** 
		- State where car is going in the right direction
		- State where car is in a collision is a low reward
	- We want to maximize reward over all time, not just greedily!
## Definitions
- Markov decision process (MDP)
	- Named after Andrey Markov
	- Popularized by Richard Bellman
	- Extends a Markov chain
	- $\mathcal{M} = \{ \mathcal{S}, \mathcal{A}, \mathcal{T}, r \}$
	- $\mathcal{S} =$ state space
		- States $s \in \mathcal{S}$
		- Can be discrete or continuous
			- **Ex.** 
				- Position of car on road
				- Images observed by car's camera
					- **NOTE:** Must be Markovian -> knowing current image is sufficient to predict future
	- $\mathcal{A} =$ action space
		- Actions $a \in \mathcal{A}$
		- Can be discrete or continuous
	- $r =$ reward function
		- $r: \mathcal{S} \times \mathcal{A} \to \mathbb{R}$
- ![[Pasted image 20250405134130.png]]
	- $p(s_{t+1}\mid s_{t}, a_{t})$
		- Transition operator or transition probability or dynamics (all synonyms)
- Partially observed Markov decision process
	- Introduces observations into the MDP
	- $\mathcal{M} = \{ \mathcal{S}, \mathcal{A}, \mathcal{O}, \mathcal{T}, \mathcal{E}, r \}$
	- $\mathcal{S} =$ state space
		- States $s \in \mathcal{S}$
		- Can be discrete or continuous
	- $\mathcal{A} =$ action space
		- Actions $a \in \mathcal{A}$
		- Can be discrete or continuous
	- $\mathcal{O} =$ observation space
		- Observations $o \in \mathcal{O}$
		- Can be discrete or continuous
	- $\mathcal{T} =$ transition operator
	- $\mathcal{E} =$ emission probability $p(o_{t} \mid s_{t})$
	- $r =$ reward function
		- $r: \mathcal{S} \times \mathcal{A} \to \mathbb{R}$
	- ![[Pasted image 20250405134247.png]]
## Goal of reinforcement learning
- ![[Pasted image 20250405135229.png]]
- Creates a distribution over the sequences of states and actions
	- $$\Huge \underbrace{ p_{\theta}(s_{1}, a_{1}, \dots, s_{T}, a_{T}) }_{ p_{\theta}(\tau) } = p(s_{1}) \prod_{t = 1}^T \underbrace{ \pi_{\theta}(a_{t} \mid s_{t}) p(s_{t + 1} \mid s_{t}, a_{t}) }_{ \text{Markov chain on } (s, a) }$$
	- $\tau =$ "trajectory"
- Objective of reinforcement learning
	- $$\huge \theta^* = \arg \underset{ \theta }{ \max } \mathbb{E}_{\tau \sim p_{\theta}(\tau)}\left[ \sum_{t} r(s_{t}, a_{t}) \right]$$
	- We want to choose parameters $\theta$ that maximize the expected value of the total rewards over all trajectories $\tau$
# Wall of math time (policy gradients)
## Evaluating the objective
- $$\huge \theta^* = \arg \underset{ \theta }{ \max } \underbrace{ \mathbb{E}_{\tau \sim p_{\theta}(\tau)}\left[ \sum_{t} r(s_{t}, a_{t}) \right] }_{ J(\theta) }$$
- How can we estimate the expected value of future rewards?
- $$\huge \frac{1}{N} \sum_{i} \sum_{t} r(s_{i,t}, a_{i, t})$$
- $$\huge J(\theta) = \mathbb{E}_{\tau \sim p_{\theta}(\tau)} \left[  \sum_{t} r(s_{t}, a_{t})  \right] \approx \frac{1}{N} \sum_{i} \sum_{t} r(s_{i, t}, a_{i, t})$$
	- We can approximate the expected value by summing over samples from $\pi_{\theta}$
	- This means running the 
		- ![[Pasted image 20250405140738.png]]
## Direct policy differentiation
- $$\huge \theta^* = \arg \underset{ \theta }{ \max } \underbrace{ \mathbb{E}_{\tau \sim p_{\theta}(\tau)}\left[ \sum_{t} r(s_{t}, a_{t}) \right] }_{ J(\theta) }$$
- We want to do gradient "ascent" -> first need the gradient
	- **NOTE:** $p_{\theta}(\tau) = \pi_{\theta}(\tau)$
	- $$\huge J(\theta) = \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)}[r(\tau)] = \int \pi_{\theta}(\tau) r(\tau) d\tau$$
		- $r(\tau) = \sum_{t = 1}^T r(s_{t}, a_{t})$
- A convenient identity
	- $$\huge \pi_{\theta}(\tau) \nabla_{\theta} \log \pi_{\theta}(\tau) = \pi_{\theta}(\tau) \frac{\nabla_{\theta} \pi_{\theta}(\tau)}{\pi_{\theta}(\tau)} = \nabla_{\theta} \pi_{\theta}(\tau)$$
- Evaluating the derivative
- $$\large \begin{align}
\nabla_{\theta} J(\theta) = \int \nabla_{\theta} \pi_{\theta}(\tau) r(\tau) d\tau = \int \underbrace{ \pi_{\theta}(\tau) \nabla_{\theta} \log \pi_{\theta}(\tau) }_{ \text{from identity} } r(\tau) d\tau  = \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)}[\nabla_{\theta} \log \pi_{\theta}(\tau)]
\end{align}$$
