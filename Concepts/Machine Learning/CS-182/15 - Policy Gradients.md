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
- $\pi_{\theta}(a_{t} | s_{t})$ is a policy
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
	- Extends a Markov chain (models a sequence of possible events) by including actions
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
	- $\mathcal{T} = p(s_{t+1}\mid s_{t}, a_{t})$
		- Transition operator or transition probability or dynamics (all synonyms)
	- $r =$ reward function
		- $r: \mathcal{S} \times \mathcal{A} \to \mathbb{R}$
- ![[Pasted image 20250405134130.png]]
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
		- Getting an observation is stochastic
	- $r =$ reward function
		- $r: \mathcal{S} \times \mathcal{A} \to \mathbb{R}$
	- ![[Pasted image 20250405134247.png]]
## Goal of reinforcement learning
- ![[Pasted image 20250405135229.png]]
- Policy -> $\pi_{\theta}(a \mid s)$
	- Probability of $s
- Our policy $\pi_{\theta}(a | s)$ creates a distribution over the sequences of states and actions
	- $$\Huge \underbrace{ p_{\theta}(s_{1}, a_{1}, \dots, s_{T}, a_{T}) }_{ p_{\theta}(\tau) } = p(s_{1}) \prod_{t = 1}^T \underbrace{ \pi_{\theta}(a_{t} \mid s_{t}) p(s_{t + 1} \mid s_{t}, a_{t}) }_{ \text{Markov chain on } (s, a) }$$
	- $\tau =$ "trajectory"
	- $s_{t}, a_{t} =$ individual state + action at a step $t$ within a specific trajectory
	- $p_{\theta}(s_{1}, a_{1}, \dots, s_{T}, a_{T}) =$ probability of a getting a sequence or "trajectory" of states and actions
		- This is a distribution of trajectories we can get from our policy
- Objective of reinforcement learning
	- $$\huge \theta^* = \arg \underset{ \theta }{ \max } \mathbb{E}_{\tau \sim p_{\theta}(\tau)}\left[ \sum_{t} r(s_{t}, a_{t}) \right]$$
	- We want to choose parameters $\theta$ that maximize the expected value of the total rewards over all trajectories $\tau$ that we can get from our policy $\pi_{\theta}$
		- The trajectories are sampled from the distribution of trajectories that the policy creates -- which is $p_{\theta}(\tau)$
# Wall of math time (policy gradients)
## Evaluating the objective
- $$\huge \theta^* = \arg \underset{ \theta }{ \max } \underbrace{ \mathbb{E}_{\tau \sim p_{\theta}(\tau)}\left[ \sum_{t} r(s_{t}, a_{t}) \right] }_{ J(\theta) }$$
- How can we estimate the expected value of future rewards?
- $$\huge \frac{1}{N} \sum_{i} \sum_{t} r(s_{i,t}, a_{i, t})$$
	- We can calculate the expectation by sampling $N$ samples, and then using the formula above (see definition of expected value)
		- This creates an "unbiased estimator" of the expected value
- $$\huge J(\theta) = \mathbb{E}_{\tau \sim p_{\theta}(\tau)} \left[  \sum_{t} r(s_{t}, a_{t})  \right] \approx \frac{1}{N} \sum_{i} \sum_{t} r(s_{i, t}, a_{i, t})$$
	- We can approximate the expected value by summing over samples from $\pi_{\theta}$
	- This means running the policy multiple times and collecting data on each trajectory 
		- ![[Pasted image 20250405140738.png]]
## Direct policy differentiation
- $$\huge \theta^* = \arg \underset{ \theta }{ \max } \underbrace{ \mathbb{E}_{\tau \sim p_{\theta}(\tau)}\left[ \sum_{t} r(s_{t}, a_{t}) \right] }_{ J(\theta) }$$
- We want to do gradient "ascent" -> first need the gradient
	- **NOTE:** $p_{\theta}(\tau) = \pi_{\theta}(\tau) =$ probability of getting a specific trajectory
	- $$\huge J(\theta) = \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)}[r(\tau)] = \int \pi_{\theta}(\tau) r(\tau) d\tau$$
		- $r(\tau) = \sum_{t = 1}^T r(s_{t}, a_{t})$
			- We can rewrite the reward over trajectories as $r(\tau)$
		- The expected value is just integral over all probabilities times the reward from that probability
			- Think of this as summing over the rewards of each trajectory weighted by that trajectory's likelihood
				- This is just definition of expected value!
### Finding $\nabla_{\theta} J(\theta)$
- We first take the derivative of both sides
	- $$\large \begin{align}
	\nabla_{\theta} J(\theta) &= \int \nabla_{\theta} \pi_{\theta}(\tau) r(\tau) d\tau  \\ &= \int \underbrace{ \pi_{\theta}(\tau) \nabla_{\theta} \log \pi_{\theta}(\tau) }_{ \text{from identity} } r(\tau) d\tau  \\ &= \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)}[\nabla_{\theta} \log \pi_{\theta}(\tau) r(\tau)]
	\end{align}$$
	- Explain
		- Step 1 -> Take the derivative (gradient) of the left and right hand sides
		- Step 2
			- Recall that the derivative of $\log_{a}$ is
				- $\dfrac{d}{dx} \log_{a} x = \dfrac{1}{x \ln a}$
			- Since it's common to use $\log x$ to refer to $\log_{e} x = \ln(x)$, that means
				- $\dfrac{d}{dx} \log x = \dfrac{d}{dx} \ln x = \dfrac{1}{x \ln e} = \dfrac{1}{x}$
			- Therefore this identity is true
				- $\pi_{\theta}(\tau) \nabla_{\theta} \log \pi_{\theta}(\tau) = \pi_{\theta} (\tau) \dfrac{\nabla_{\theta} \pi_{\theta} (\tau)}{\pi_{\theta} (\tau)} = \nabla_{\theta} \pi_{\theta} (\tau)$
			- We use this identity in reverse to substitute $\nabla_{\theta}\pi_{\theta}(\tau) = \pi_{\theta}(\tau) \nabla_{\theta} \log \pi_{\theta}(\tau)$
		- Step 3
			- Recall that
				- $\displaystyle\int \pi_{\theta}(\tau) \dots d\tau = \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)}[\dots]$
			- Therefore, we can rewrite the equation into an expected value of $\nabla_{\theta} \log \pi_{\theta}(\tau)$
			- This effectively tells us we can calculate the gradient of our unbiased estimator by summing together and averaging the gradients of each sample
- To get $\log \pi_{\theta}(\tau)$
	- $$\large \begin{align}
	\underbrace{ \pi_{\theta}(s_{1}, a_{1}, \dots, s_{T}, a_{T}) }_{ \pi_{\theta}(\tau) } &= p(s_{1}) \prod_{t = 1}^T \pi_{\theta}(a_{t} \mid s_{t}) p(s_{t + 1} \mid s_{t}, a_{t}) \\
	\log \pi_{\theta}(\tau) &= \log p(s_{1}) + \sum_{t=1}^T \log \pi_{\theta}(a_{t} \mid s_{t}) + \log p(s_{t+1} \mid s_{t},a_{t}) \\
	\end{align}$$
- Therefore,
	- $$\large \begin{align}
	\nabla_{\theta} \log \pi_{\theta}(\tau) &= \nabla_{\theta} \left[  \log p(s_{1}) + \sum_{t = 1}^T \log \pi_{\theta} (a_{t} \mid s_{t}) + \log p(s_{t + 1} \mid s_{t}, a_{t}) \right] \\
	&= \nabla_{\theta} \sum_{t = 1}^T \log \pi_{\theta} (a_{t} \mid s_{t}) \\
	&= \sum_{t = 1}^T \nabla_{\theta} \log \pi_{\theta} (a_{t} \mid s_{t}) \\
	\end{align}$$
	- We can eliminate all terms except for $\sum_{t = 1}^T \log \pi_{\theta}(a_{t \mid s_{t}})$, because that's the only term that depends on $\theta$, and would affect the derivative
		- The others are treated as constants, and would be $0$ in the derivative
- Now, we can expand $\nabla_{\theta} J(\theta)$ to
	- $$\large\begin{align}
\nabla_{\theta} J(\theta) &= \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)} [ \nabla_{\theta} \log \pi_{\theta} (\tau) r(\tau)] \\
 &= \mathbb{E}_{\tau\sim \pi_{\theta}(\tau)} \left[ \left( \sum_{t = 1}^T \nabla_{\theta} \log \pi_{\theta} (a_{t} \mid s_{t}) \right) \left( \sum_{t=1}^T r(s_{t}, a_{t}) \right)\right]\\
\end{align}$$
## Evaluating the policy gradient
- $$\huge\begin{align}
\nabla_{\theta} J(\theta) = \mathbb{E}_{\tau\sim \pi_{\theta}(\tau)} \left[ \left( \sum_{t = 1}^T \nabla_{\theta} \log \pi_{\theta} (a_{t} \mid s_{t}) \right) \left( \sum_{t=1}^T r(s_{t}, a_{t}) \right)\right]\\
\end{align}$$
- Steps
	1. Generate trajectories (samples) from $\pi_{\theta}(\tau)$
	2. For each trajectory
		- Calculate the sum of the rewards $r(s_{t}, a_{t})$ along the trajectory -> first term in equation
		- Calculate total of the $\nabla_{\theta} \log \pi_{\theta}(a_{t} \mid s_{t})$ at each timestep along the trajectory -> second term in equation
		- Multiple the first and second terms together to get a value
	3. Average the "values" we get from each trajectory to get $J(\theta)$
	4. Apply gradient ascent to modify $\theta$ in the direction that leads to a higher reward
		- $\theta \leftarrow \theta + \alpha \nabla_{\theta} J(\theta)$
- AKA the **REINFORCE** algorithm
- AKA policy gradient algorithm
- AKA likelihood ratio policy gradient algorithm
- ![[Pasted image 20250717144528.png]]
## Comparison to maximum likelihood
- Policy gradient
	- $$\huge \nabla_{\theta} J(\theta) \approx \frac{1}{N} \sum_{i = 1}^N \left( \sum_{t = 1}^T  \nabla_{\theta} \log \pi_{\theta}(a_{i, t} \mid s_{i, t})\right) \left( \sum_{t = 1}^T r(s_{i,t}, a_{i,t})\right)$$
- Maximum likelihood gradient
	- $$\huge \nabla_{\theta} J_{\text{ML}}(\theta) \approx \frac{1}{N} \sum_{i = 1}^N \left( \sum_{t = 1}^T \nabla_{\theta} \log \pi_{\theta}(a_{i,t} \mid s_{i,t}) \right)$$
- The policy gradient looks just like the [[2 - Machine Learning Basics#^maximum-log-likelihood|maximum likelihood gradient]] 
	- Difference is that in each trajectory, the log likelihood of the trajectory is multiplied by the total reward along the trajectory
## What did we just do?
- Let's simplify the policy gradient and maximum likelihood gradient formulas, by talking about them both in the context of trajectories, rather than individual state action pairs
- Policy gradient
	- $$\large \begin{align}
	\nabla_{\theta} J(\theta) &\approx \frac{1}{N} \sum_{i = 1}^N \left( \sum_{t = 1}^T  \nabla_{\theta} \log \pi_{\theta}(a_{i, t} \mid s_{i, t})\right) \left( \sum_{t = 1}^T r(s_{i,t}, a_{i,t})\right) \\
	\nabla_{\theta} J(\theta) &\approx \frac{1}{N} \sum_{i = 1}^N \nabla_{\theta} \log \pi_{\theta}(\tau_{i}) r(\tau_{i})
	\end{align}$$
	- This gradient is the direction that maximizes the likelihood of the actions along each trajectory that provide the highest reward
		- Actions that have no reward (0), or even negative reward (-1), will cause the gradient to move in the opposite direction
	- Think of the $r(s_{i,t}, a_{i,t})$ as a weighting factor for each action taken along a trajectory
		- If the action provided good reward, then we want the gradient to make $\theta$ prioritize that action
	- Informally
		- This algorithm is mathematical formalization of "trial and error"
		- Makes good stuff more likely, and bad stuff less likely
	- ![[Pasted image 20250717150253.png]]
- Maximum likelihood gradient
	- $$\large \nabla_{\theta}J_{\text{ML}}(\theta) \approx \frac{1}{N}\sum_{i =1}^N \nabla_{\theta} \log \pi_{\theta} (\tau_{i})$$
	- This gradient is the direction that maximizes the likelihood of all the action along each trajectory
	- ![[Pasted image 20250717150233.png]]
## Partial observability
- Vanilla policy gradient does not care about partial observability
- Can plug in observations for states $o_{i,t} = s_{i,t}$
- This is because the Markov property is not used in vanilla policy gradients
## Making policy gradient actually work
## A better estimator
- Our current estimator
	- $$\large \nabla_{\theta} J(\theta) \approx \frac{1}{N} \sum_{i = 1}^N \left( \sum_{t = 1}^T  \nabla_{\theta} \log \pi_{\theta}(a_{i, t} \mid s_{i, t})\right) \left( \sum_{t = 1}^T r(s_{i,t}, a_{i,t})\right)$$
- This is estimator does not account for causality, because each action is multiplied against the reward of the entire trajectory, rather than only the rewards of future steps starting at the action
- Causality -> policy at time $t'$ cannot affect reward at time $t$ when $t < t'$
- An improved estimator
	- $$\large \nabla_{\theta} J(\theta) \approx \frac{1}{N} \sum_{i = 1}^N \sum_{t = 1}^T  \nabla_{\theta} \log \pi_{\theta}(a_{i, t} \mid s_{i, t}) \underbrace{ \left( \sum_{t' = t}^T r(s_{i,t}, a_{i,t})\right) }_{ \huge \text{reward to go} }$$
	- We now only sum from the current step $t$ to the end of the trajectory $T$ (ignore the past)
	- In expectation, the influence of past rewards integrates to zero
	- But for finite sample sizes, we can get sampling error
- Proof
- $$\begin{align}
J_{\theta}(\theta) &= \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)} \left[  \left(  \sum_{t = 1}^T \nabla_{\theta} \log \pi_{\theta}(a_{t} \mid s_{t}) \right) \left( \sum_{t = 1}^T r(s_{t}, a_{t}) \right) \right] \\
&= \sum_{t = 1}^T \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)} \left[  \nabla_{\theta} \log \pi_{\theta}(a_{t} \mid s_{t}) \sum_{t' = 1}^T r(s_{t}, a_{t}) \right] \\
&=
\end{align}$$
- Here are the identities/lemmas/theorems used in the previous proof
	- [Expected Value of a Random Variable](https://www.youtube.com/watch?v=-7QG2itL1u4&t=422s)
		- Let $x \in X$, and $P(x) =$ probability of getting outcome $x$ from the random variable $X$
		- $\large \displaystyle \mathbb{E}[X] = \int P(x) x \; dx$
	- [Linearity of Expectation](https://ocw.mit.edu/courses/6-042j-mathematics-for-computer-science-fall-2005/6ad0342f836f80c219470870db432c18_ln14.pdf)
		- $\large \displaystyle \mathbb{E} \left[ \sum_{i} X \right] = \sum_{i} \mathbb{E}\left[X\right]$
		- Let $x \in X$, and $P(x) =$ probability of getting outcome $x$ from the random variable $X$
		- $$\large \begin{align}
			\mathbb{E}_{\tau} \left[  \sum_{i} X \right] &= \int P(x)\sum_{i} x \; dx \\
			&= \sum_{i} \int P(x) x \; dx \\
			&= \sum_{i} \mathbb{E}[X]
			\end{align}$$
		- Note that this only works as long as the summation $\sum_{i}$ is finite
	- [Expected Grad-Log-Prob Lemma](https://spinningup.openai.com/en/latest/spinningup/rl_intro3.html#expected-grad-log-prob-lemma)^expected-grad-log-prob-lemma
		- $\large \displaystyle \sum_{t=1}^T \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)}\left[ \nabla_{\theta} \log \pi_{\theta}(a_{t} \mid s_{t}) \right] =\mathbb{E}_{\tau \sim \pi_{\theta}(\tau)}\left[ \nabla_{\theta} \log \pi_{\theta}(\tau) \right] = 0$
		- As proved below:
		- $$\large \begin{align}
			\mathbb{E}[\nabla_{\theta} \log \pi_{\theta}(\tau)] &= \int \pi_{\theta}(\tau) \nabla_{\theta} \log \pi_{\theta}(\tau) d\tau \\
			&= \int \nabla_{\theta} \log \pi_{\theta}(\tau) d\tau \\
			&= \nabla_{\theta} \int \log \pi_{\theta}(\tau) d\tau \\
			&= \nabla_{\theta} (1) \\
			&= 0
			\end{align}$$
## Baselines
$$\huge \nabla_{\theta} J(\theta) \approx \frac{1}{N} \sum_{i = 1}^N \nabla_{\theta} \log \pi_{\theta}(\tau) r(\tau)$$
- If the rewards of all trajectories have large numbers and small variance, this gradient would always be positive
	- Therefore, it would make all trajectories more likely
- Instead, we want to subtract the average from the rewards
	- $$\huge \begin{align}
	\nabla_{\theta} J(\theta) &\approx \frac{1}{N} \sum_{i = 1}^N \nabla_{\theta} \log \pi_{\theta}(\tau) [r(\tau) - b] \\
	b &= \frac{1}{N} \sum_{i=1}^N r(\tau)
	\end{align}$$
- This is allowed, because $\mathbb{E}[\nabla_{\theta} \log \pi_{\theta}(\tau)] = 0$ according to the [[#^expected-grad-log-prob-lemma|Expected Grad-Log-Prob Lemma]]
	- $$\large \begin{align}
	\mathbb{E}_{\tau\sim \pi_{\theta}(\tau)}[\nabla_{\theta} \log \pi_{\theta}(\tau)[r(\tau) - b]] &= \mathbb{E}_{\tau \sim \pi_{\theta (\tau)}}[\nabla_{\theta} \log \pi_{\theta}(\tau) r(\tau)] - \mathbb{E}_{\tau \sim \pi_{\theta (\tau)}}[\nabla_{\theta} \log \pi_{\theta}(\tau) b] \\
	&= \mathbb{E}_{\tau \sim \pi_{\theta (\tau)}}[\nabla_{\theta} \log \pi_{\theta}(\tau) r(\tau)] - b \mathbb{E}_{\tau \sim \pi_{\theta (\tau)}}[\nabla_{\theta} \log \pi_{\theta}(\tau)] \\
	&= \mathbb{E}_{\tau \sim \pi_{\theta (\tau)}}[\nabla_{\theta} \log \pi_{\theta}(\tau) r(\tau)] - b (0) \\
	&= \mathbb{E}_{\tau \sim \pi_{\theta (\tau)}}[\nabla_{\theta} \log \pi_{\theta}(\tau) r(\tau)]
	\end{align}$$
- Subtracting a baseline is unbiased in expectation!
- Average reward is not the best baseline, but it's pretty good!
## Policy gradient is on-policy
- $\theta^* = \arg \underset{ \theta }{ \max } J(\theta)$
- $J(\theta) = \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)} [ r(\tau)]$
- $\nabla_{\theta} J(\theta) = \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)} [ \nabla_{\theta} \log \pi_{\theta}(\tau) r(\tau)]$
- REINFORCE algorithm^reinforce-algorithm
	1. Sample ${\tau^i}$ from $\pi_{\theta}(a_{t} \mid s_{t})$ (run it on the robot)
	2. $\nabla_{\theta} J(\theta) = \sum_{i} \left( \sum_{t} \nabla_{\theta} \log \pi_{\theta}(a_{t}^i \mid s_{t}^i) \right) (\sum_{t} r(s_{t}^i, a_{t}^i))$
	3. $\theta \leftarrow \theta + \alpha \nabla_{\theta}J(\theta)$
- Neural networks change only a little bit with each gradient step
- Each step we must resample from the policy, since the policy — and it's trajectory distribution — have changed
- On-policy learning can be extremely inefficient
## Off-policy learning & importance sampling
- $\theta^* = \arg \underset{ \theta }{ \max } J(\theta)$
- $J(\theta) = \mathbb{E}_{\tau \sim \pi_{\theta}(\tau)} [ r(\tau)]$
- Importance sampling
	- Estimating expected value of a function under a distribution, given only samples from a different distribution
	- $$\begin{align}
	\mathbb{E}_{x \sim p(x)}[f(x)] &= \int p(x) f(x) \; dx \\
	&= \int \frac{q(x)}{q(x)} p(x) f(x) \; dx \\
	&= \int q(x) \frac{p(x)}{q(x)} f(x) \; dx \\
	&= \mathbb{E}_{x \sim q(x)} \left[  \frac{p(x)}{q(x)} f(x) \right]
	\end{align}$$
	- $\displaystyle\frac{p(x)}{q(s)}$ is known as the "importance weight"
- What if we don't have samples from $\pi_{\theta}(\tau)$
	- We have samples from $\bar{\pi}(\tau)$ instead
	- $\displaystyle J(\theta) = \mathbb{E}_{\tau \sim \bar{\pi} (\tau)} \left[\frac{\pi_{\theta}(\tau)}{\bar{\pi}(\tau)} r(\tau)\right]$
	- Recall that
		- $\displaystyle \pi_{\theta}(\tau) = p(s_{1}) \prod_{t=1}^T \pi_{\theta}(a_{t} \mid s_{t})$