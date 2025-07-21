# Probability [^1]
- **Probability distribution** ^probability-distribution
	- A function $\mathbb{P}$ that assigns a real number a real number $\mathbb{P}(A)$ to every event $A$ is a **probability distribution or probability measure** if it satisfies the three axioms
		1. $\mathbb{P}(A) \geq 0 \text{ for every } A$
			- Probability of every event is $\geq$ 0
		2. $\mathbb{P}(\Omega) = 1$
			- Probability of all outcomes is 1
		3. If $A_{1}, A_{2}, \dots$ are disjoint, then
			- $$\large \mathbb{P}\left( \bigcup_{i=1}^\infty A_{i} \right) = \sum_{i =1}^\infty \mathbb{P}(A_{i})$$
	- $\mathbb{P}(A)$
		- Probability of an event $A$
## Interpretations of $\mathbb{P}(A)$
- There are two common interpretations of $\mathbb{P}(A)$
	- Frequency interpretation
		- AKA frequentist interpretation
		- $\mathbb{P}(A)$ is the proportion of times $A$ is true in the long-run through many repetitions
			- **Ex.** If probability of heads is 1/2, then in the long run over many trials, we expect around half of all the trials to be heads 
	- Degree-of-belief interpretation
		- AKA Bayesian interpretation
		- $\mathbb{P}(A)$ measures an observer's strength of the belief that $A$ is true
			- **Ex.** If the probability of raining tomorrow is 0.9, then it's very likely it'll rain tomorrow.
- These two beliefs are part of a greater frequentist vs Bayesian difference in statistical inference
## Properties
- $$\large \begin{align}
&\mathbb{P}(\emptyset) = 0 \\
&A \subset B \implies \mathbb{P}(A) \leq \mathbb{P}(B) \\
&0 \leq \mathbb{P}(A) \leq 1 \\
&\mathbb{P}(A^C) = 1 - \mathbb{P}(A) \\
&A \cap B = \emptyset \implies \mathbb{P}(A \cup B) = \mathbb{P}(A) + \mathbb{P}(B)
\end{align}$$
- **Union** ^union
	- Probability of at least one of two events occurring is
		- $$\large \mathbb{P}(A \cup B) = \mathbb{P}(A) + \mathbb{P}(B) - \mathbb{P}(AB)$$
	- **Ex.** Consider two coin tosses, with $H_{1}$ be event that heads occurs on toss 1, and $H_{2}$ be event that heads occurs on toss 2. If all outcomes are equally likely, then
		- $$\begin{align}
		\mathbb{P}(H_{1} \cup H_{2}) &= \mathbb{P}(H_{1}) + \mathbb{P}(H_{2}) - \mathbb{P}(H_{1} \cap H_{2})  \\
		&= \frac{1}{2} + \frac{1}{2} - \frac{1}{4} = \frac{3}{4}
		\end{align}$$
- **Intersection** ^intersection
	- AKA joint probability
	- The probability of two events occurring together
		- **Ex.** The probability that it 
	- $\mathbb{P}(A \cap B) = \mathbb{P}(A, B)$
		- Two types of notation for the probability of event $A$ and event $B$ happening together
	- The joint probability of two events can be calculating using the [[#^chain-rule|chain rule]]
		- $$\large \mathbb{P}(A \cap B) = \mathbb{P}(B) \mathbb{P}(A \mid B)$$
- **THOREM:** Continuity of Probabilities ^continuity-probabilities
	- If $A_{n} \to A$, then $\mathbb{P}(A_{n}) \to \mathbb{P}(A)$ as $n \to \infty$
	- **NOTE:**
		- $A_{n} \to A$ means the set $A_{n}$ converges to the set $A$

[^1]: [[wasserman_all_of_stats.pdf]]
