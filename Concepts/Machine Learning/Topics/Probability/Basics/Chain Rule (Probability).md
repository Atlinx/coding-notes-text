# Chain Rule (Probability)[^1]
- AKA general product rule
- Calculates the probability of the [[#^joint|intersection]] of events, which may not necessarily be [[#^independent|independent]]
- For two events
	- $$\large \begin{align}
	P(A \cap B) &= P(B) P(A \mid B)
	\end{align}$$
	- The probability of $A$ and $B$ occurring
- For $n$ events
	- $$\large \begin{align}
	P(A_{1} \cap A_{2} \cap A_{3} \cap \dots) &= P(A_{1}) P(A_{2} \mid A_{1}) P(A_{3} \mid A_{1}, A_{2}) \dots
	\end{align}$$
	- Each additional event $A_{i}$ in the joint distribution is conditioned on the previous events ($A_{1}, \dots, A_{i-1}$)
- For $n$ independent events
	- $$\large \begin{align}
	P(A_{1} \cap A_{2} \cap A_{3} \cap \dots) &= P(A_{1}) P(A_{2}) P(A_{3}) \dots
	\end{align}$$
	- If the events are [[#^independent|independent]] from one another, then $P(A_{i} \mid A_{1}, \dots, A_{i-1}) = P(A_{i})$

[^1]: https://en.wikipedia.org/wiki/Chain_rule_(probability)
