# 16 - Actor-Critic & Q-Learning
## Recap: policy gradients
![[Pasted image 20250719201535.png]]
## Improving the policy gradient
- $$\huge \nabla_{\theta}J(\theta) \approx \frac{1}{N} \sum_{i = 1}^N \sum_{t = 1}^T \nabla_{\theta} \log \pi_{\theta}(a_{i,t} \mid s_{i,t}) \underbrace{ \left( \sum_{t' = 1}^T r(s_{i,t'}, a_{i,t'}) \right) }_{ \Huge \text{``reward to go"} \; = \; \hat{Q}_{i,t} }$$
- $\hat{Q}_{i,t} =$ estimate of expected reward if we take action $a_{i,t}$ in state $s_{i,t}$
	- Problem -> Our current reward is the future rewards along the same trajectory
		- However, there can be multiple valid trajectories from the current state
		- Our current solution is called a "single sample estimator"
			- We are estimating an integral using a single sample
			- It's still valid estimator, but it requires more samples
		- ![[Pasted image 20250719201944.png]]
- Can we get a better estimate?
	- $$\huge Q(s_{t}, a_{t}) = \sum_{t' = t}^T \mathbb{E}_{\pi_{\theta}}[r(s_{t'}, a_{t'}) \mid s_{t}, a_{t}]$$
		- True expected reward-to-go
	- $$\huge \nabla_{\theta}J(\theta) \approx \frac{1}{N} \sum_{i = 1}^N \sum_{t=1}^T \nabla_{\theta} \log \pi_{\theta}(a_{i,t} \mid s_{i,t}) Q(s_{i,t}, a_{i,t})$$
## What about the baseline?
- $$\huge Q(s_{t}, a_{t}) = \sum_{t' = t}^T \mathbb{E}_{\pi_{\theta}}[r(s_{t'}, a_{t'}) \mid s_{t}, a_{t}]$$
	- True expected reward to go
- Recall that we can add a base-line to our gradient
	- $$\large \nabla_{\theta}J(\theta) \approx \frac{1}{N} \sum_{i=1}^N \sum_{t=1}^T \nabla_{\theta} \log \pi_{\theta}(a_{i,t} \mid s_{i,t})(Q(s_{i,t}, a_{i,t}) - b)$$
	- $\large \displaystyle b_{t} = \frac{1}{N} \sum_{i} Q(s_{i,t}, a_{i,t})$
	- $b_{t}$ is the average of all of the rewards
- We can rewrite this $b$ as a new function $V(s_{i})$
	- $$\huge V(s_{t}) = \mathbb{E}_{a_{t} \sim \pi_{\theta}(s_{t} \mid s_{t})}[Q(s_{t}, a_{t})]$$
	- This is the "value" of being at the state $s_{t}$
- We can now use $V(s_{t})$ in our gradient 
	- $$\huge \nabla_{\theta}J(\theta) \approx \frac{1}{N} \sum_{i=1}^N \sum_{t=1}^T \nabla_{\theta} \log \pi_{\theta}(a_{i,t} \mid s_{i,t})(Q(s_{i,t}, a_{i,t}) - V(s_{i,t}))$$
- We call the new term "advantage"^advantage
	- $$\huge A(s_{i,t}, a_{i,t}) = Q(s_{i,t}, a_{i,t}) - V(s_{i,t})$$
	- Advantage describes how much better is the action $a$ we took at state $s$, compared to the average action
## State & state-action value functions
- $\large Q^\pi(s_{t}, a_{t}) = \sum_{t'=t}^T \mathbb{E}_{\pi_{\theta}}[r(s_{t'}, a_{t'})\mid s_{t}, a_{t}]$
	- Total reward form taking $a_{t}$ in $s_{t}$ for a particular policy $\pi_{\theta}$ at timestep
	- "Expected reward" function
- $\large V^\pi(s_{t}) = \mathbb{E}_{a_{t} \sim \pi_{\theta}(a_{t} \mid s_{t})} [Q^\pi (s_{t}, a_{t})]$
	- Total reward from $s_{t}$
	- That is, all the expected value of every possible action we can take from state $s_{t}$ under our policy $\pi_{\theta}$
	- "Value" function
- $\large A^\pi(s_{t},a_{t}) = Q^\pi(s_{t},a_{t}) - V^\pi(s_{t})$
	- How much better a specific action $a_{t}$ is from the average of all the possible actions at $s_{t}$
- Policy gradient with advantage
	- $$\huge \nabla_{\theta}J(\theta) \approx \frac{1}{N} \sum_{i=1}^N \sum_{t=1}^T \nabla_{\theta} \log \pi_{\theta}(a_{i,t} \mid s_{i,t})A^\pi(s_{i,t}, a_{i,t})$$
		- The better the estimate of $A^\pi$, the lower the variance of the policy gradient
- Old policy gradient
	- $$\huge \nabla_{\theta}J(\theta) \approx \frac{1}{N} \sum_{i=1}^N \sum_{t=1}^T \nabla_{\theta} \log \pi_{\theta}(a_{i,t} \mid s_{i,t}) \left( \sum_{t' = 1}^T r(s_{i,t'}, a_{i,t'}) - b \right)$$
	- Unbiased, but high variance single-sample estimate
	- We only compute the future rewards along a single trajectory
- We want to fit a different neural network to estimate either $Q$, $V$, or $A$
	- ![[Pasted image 20250719204216.png]]
## Value function fitting
- $$\huge \begin{align}
Q^\pi (s_{t}, a_{t}) &= \sum_{t'=t}^T \mathbb{E}_{\pi_{\theta}}[r(s_{t'}, a_{t'})\mid s_{t}, a_{t}] \\
&=r(s_{t}, a_{t}) + \sum_{t' = t+1}^T \mathbb{E}_{\pi_{\theta}} [r(s_{t'},a_{t'}) \mid s_{t}, a_{t}] \\
&= r(s_{t}, a_{t}) + V^\pi(s_{t+1}) \\
\end{align}$$
	- We can rewrite $Q$ as the current reward at state $s_{t}$ using action $a_{t}$, plus the sum of expected future rewards
	- Then, we can rewrite the future rewards as the value of the next state $s_{t+1}$ that we transition to using action $a_{t}$
	- The final equation is also known as the Bellman-equation^bellman-equation
- We can rewrite the $Q^\pi$ and $A^\pi$ in terms of the value function $V^\pi$
	- $Q^\pi(s_{t},a_{t}) \approx r(s_{t}, a_{t}) + V^\pi(s_{t+1})$
	- $A^\pi(s_{t},a_{t}) \approx r(s_{t}, a_{t}) + V^\pi(s_{t+1}) - V^\pi(s_{t})$
- Classic actor-critic methods fit the value function, since it only depends on the state
## Policy evaluation
- Fitting the value function is also known as "policy evaluation"
- $J(\theta) = \mathbb{E}_{s_{1} \sim p(s_{1})}[V^\pi(s_{1})]$
	- Recall that the objective function is the expected reward from the initial state $s_{1}$
	- This can be expressed in terms of the value function
- How can we perform policy evaluation?
	- Monte Carlo policy evaluation -> generate samples from policy, run, and see what total reward is
		- $\large \displaystyle V^\pi(s_{t}) \approx \sum_{t'=t}^T r(s_{t'}, a_{t'})$
			- If we have a single trajectory starting from $s_{t}$
			- This is a single sample estimate
		- $\large \displaystyle V^\pi(s_{t}) \approx \frac{1}{N} \sum_{i = 1}^N \sum_{t'=t}^T r(s_{t'}, a_{t'})$
			- If we have a simulator, and can re-sample from the exact same state $s_{t}$
			- This is a multi-sample estimate
## Monte Carlo evaluation with function approximation
- $\large \displaystyle V^\pi(s_{t}) \approx \sum_{t'=t}^T r(s_{t'}, a_{t'})$
- Not as good as having multiple samples from the same state
- However, it's still a good idea to learn a value function rather than using a single-sample estimate
	- For states that are adjacent to each other, the function would "average out" their expected values 
	- This reduces the effect of "lucky" outcomes, since it's averaged out with other similar states
	- ![[Pasted image 20250719213138.png]]
- We can collect state ($s_{i,t}$) and expected future reward ($y_{i,t}$) pairs, and then train mean square error regression on it
	- Training data
		- $\huge \displaystyle \Big\{\Big( s_{i,t} \;, \; \underbrace{ \sum_{t'=t}^T r(s_{i,t'}, a_{i,t'} }_{\huge  y_{i,t} }) \Big)\Big\}$
	- Supervised regression
		- $\huge \displaystyle L(\phi) = \frac{1}{2} \sum_{i} \left\lVert \hat{V}_{\phi}^\pi (s_{i} - y_{i}) \right\rVert^2$
## Can we do better?
- Ideal target
	- $$\huge \begin{align}
	y_{i,t} &= \sum_{t'=t}^T \mathbb{E}_{\pi_{\theta}}[r(s_{t'}, a_{t'}) \mid s_{i,t}]  \\
	&\approx r(s_{i,t}, a_{i,t}) + V^\pi(s_{i,t+1}) \\
	&\approx r(s_{i,t}, a_{i,t}) + \hat{V}_{\phi}^\pi (s_{i,t+1}) \\
	\end{align}$$
	- Since the definition of the value is recursive, we can approximate it using our previously fitted value function $\hat{V}_{\phi}^\pi$
- Monte Carlo target
	- $\huge \displaystyle y_{i,t} = \sum_{t'=t}^Tr(s_{i,t'}, a_{i,t'})$
- Training data
	- $\huge \displaystyle \Big\{\Big( s_{i,t} \;, \; \underbrace{ r(s_{i,t'}, a_{i,t'}) + \hat{V}_{\phi}^\pi (s_{i,t+1}) }_{ \huge y_{i,t} } \Big)\Big\}$
- Sometimes referred to as a "bootstrapped" estimate
## Policy evaluation