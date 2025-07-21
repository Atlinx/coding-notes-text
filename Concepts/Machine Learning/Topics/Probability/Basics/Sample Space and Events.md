# Sample Space and Events [^1]
- **Sample space** ^sample-space
	- A set of possible outcomes in an experiment
	- Denoted as $\Omega$
- **Outcome** ^outcome
	- Points $w$ in sample space are called **sample outcomes, realization, or elements**
- **Event** ^event
	- An event is a subset of the elements of $\Omega$
## Terminology
- **Complement** ^event-complement
	- $$\large A^C = \{ \omega \in \Omega : \omega \not\in A\}$$
	- The complement of "$A$"
	- Informally read as "not $A$" -> $A^C$ includes all outcomes not included in $A$
	- Complement of $\Omega$ is the empty set $\emptyset$
- **Union** ^event-union
	- The "OR" operation for events
	- Union of two events $A$ and $B$
		- $$\large A \cup B = \{ \omega \in \Omega :\omega \in A \text{ or } \omega \in B \text{ or } \omega \in \text{both} \}$$
		- Union of two events is defined as a new set that contains elements from both events (without duplicates).
		- Think of as "A or B"
	- Union of a sequence of events $A_{1}, A_{2}, A_{3}, \dots$ 
		- $$\large \bigcup_{i=1}^\infty A_{i} = \Big\{ \omega \in \Omega : \omega \in A_{i} \text{ for at least one } i \Big\}$$
		- Each outcome $\omega$ in the union must be in at least one of the events
- **Intersection**  ^event-intersection
	- The "AND" operation for events
	- Intersection of two events $A$ and $B$
		- $$\large A \cap B = \{ \omega \in \Omega : \omega \in A \text{ and } \omega \in B \}$$
		- Intersection of two events is a new set that whose elements exist in both events
		- Think of as "A and B"
	- Intersection of a sequence of events $A_{1}, A_{2}, A_{3}, \dots$
		- $$\large \bigcap_{i=1}^\infty A_{i} = \Big\{ \omega \in \Omega : \omega \in A_{i} \text{ for all } i \Big\}$$
		- Each outcome $\omega$ in the union must be in ALL of the events
- **Null event** ^null-event
	- An event that contains no elements
		- $A = \emptyset$
	- This event is always false
- **True event** ^true-event
	- An event that contains the entire sample space
		- $A = \Omega$
	- This event is always true
- **Disjoint** ^event-disjoint
	- Events $A_{1}, A_{2}, \dots$ are **disjoint or mutually exclusive** if $A_{i} \cap A_{j} = \emptyset$ for all $i \neq j$
	- A set of events is disjoint if their outcomes do not overlap with each other
		- **Ex.** $A_{1} = [0, 1), A_{2} = [1, 2), A_{3} = [2, 3), \dots$ are disjoint
- **Indicator function** ^indicator-function
	- The indicator function of an event $A$ is
	- $$I_{A}(\omega) = I(\omega \in A) = \begin{cases}
		1 &\text{if } \omega \in A \\
		0 &\text{if } \omega \not\in A
		\end{cases}$$
	- **NOTE:**
		- Indicator functions are a property of sets, and map the elements of a subset to one, and all other elements to zero [^2]
## Examples
- **Ex.** If we toss a coin twice, then
	- $\Omega = \{H H, H T, T H, T T\}$
	- The event that the first toss is heads is $A = \{ H H, H T\}$
- **Ex.** Let $\omega$ be the measurement of a physical quantity like temperature
	- $\Omega = \mathbb{R} = (-\infty, \infty)$
	- The event that the measurement is larger than 10 but less than or equal to 23 is
		- $A=(10, 23]$
- **Ex.** If we toss a coin forever, the sample space is a infinite set
	- $$\large \Omega = \Big\{ \omega = (\omega_{1}, \omega_{2}, \omega_{3}, \dots) : \omega_{i} \in \{ H, T\}\Big\}$$
	- Let $E$ be the event that the first head appears on the third toss
		- $$E = \Big\{ \omega = (\omega_{1}, \omega_{2}, \omega_{3}, \dots) : \omega_{1} = T, \omega_{2} = T, \omega_{3} = H, \omega_{i} \in \{H, T\} \text{ for } i > 3 \Big\}$$

[^1]: [[wasserman_all_of_stats.pdf]]

[^2]: https://en.wikipedia.org/wiki/Indicator_function
