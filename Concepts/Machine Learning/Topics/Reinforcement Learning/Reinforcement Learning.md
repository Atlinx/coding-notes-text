# Reinforcement Learning
- The goal of reinforcement learning is to get a robot/agent using actions given observations about the world
## Terminology
- State ^state
	- $s_{t}$ -> The state at timestep $t$
	- The state of the world satisfies the [[Markov Model]]
- Observation ^observation
	- $o_{t}$ -> The observation at timestep $t$
	- A view of the 
- Policy ^policy
	- $\pi_{\theta}(a_{t} \mid o_{t})$ -> The policy with parameters $\theta$ that outputs the distribution of actions given an observation $o_{t}$
	- Policy
	- Function that takes in the state as input, and outputs a distribution of actions
		- **Ex.** The output can be $[0.2, 0.3, 0.5]$, where the probability of action 1 is $0.2$, the probability of action 2 is $0.3$, and the probability of action 3 is $0.5$
- Trajectory ^trajectory
	- 
## Algorithms
- [[PPO|Proximal Policy Optimization]]
- [[Imitation Learning]]