# 2 Linear Algebra
- Algebra
	- Set of objects (symbols)
	- Set of rules to manipulate objects
- Linear algebra
	- Study of vectors and rules to manipulate vectors
	Examples of vectors
	- Geometric vectors
		- Denoted with an arrow, e.g. $\vec{x}$
		- Represented by direction and magnitude
		- $\vec{x} + \vec{y} = \vec{z}$
			- Can be added together
	- Polynomials
		- The coefficients of the polynomials can be added together, and multiplied by scalars
	- Audio signals
		- Audio signals are represented by series of numbers (array of numbers = vector)
		- Can add audio signals together -> sum is new audio signal
		- Can scale audio signals
	- Elements of $\mathbb{R}^n$ (tuples of real numbers)
		- Ex.
			- $$\mathcal{a} = \begin{bmatrix}
				1 \\ 2 \\ 3
				\end{bmatrix} \in \mathbb{R}^3$$
		- Adding two vectors component-wise results in another vector
		- Multiplying a vector $a \in \mathbb{R}^n$ by a scalar $\lambda \in \mathbb{R}$ results in a scaled vector $\lambda \mathcal{a} \in \mathbb{R}^n$
- Book focuses on vectors in $\mathbb{R}^n$ since most algorithms are formulated in $\mathbb{R}^n$
- Closure -> What is the set of all things that can result from a set of proposed operations?
	- Vector space -> What is set of vectors that can result with starting a small set of vectors, and adding them to each other and scaling them?
- Mind map
	- ![[Pasted image 20250703120523.png]]
## 2.1 Systems of Linear Equations
- **Example 2.1**
	- Company produces products $N_{1}, \dots, N_{n}$ which requires resources $R_{1}, \dots, R_{m}$ to make.
	- Producing one unit of product $N_{j}$ requires $a_{ij}$ units of resource $R_{j}$, where $i = 1, \dots, m$ (a specific product) and $j = 1, \dots, n$ (a specific resource).
	- Objective -> find an optimal production that describes how many units $x_{j}$ of product $N_{j}$ should be produced if total of $b_{i}$ units of resource $R_{i}$ are available
	- Amount of units $b_{i}$, required for a given resource $R_{i}$ is
		- $$\huge a_{i1}x_{1} + a_{i2}x_{2} + \dots + a_{in}x_{n} = b_{i}$$
		- Specifically,
			- $$\huge \underbrace{ a_{i1}x_{1} }_{ \text{amount of } R_{i} \text{ required by } x_{1}}$$
	- An optimal production plan must satisfy this system of equations
		$$\large \begin{align}
		a_{11}x_{1} + a_{12}x_{2} + \dots + a_{1n} x_{n} &= b_{1} \\
		a_{21}x_{1} + a_{22}x_{2} + \dots + a_{2n} x_{n} &= b_{2} \\
		&\vdots \\
		a_{m1}x_{1} + a_{m2}x_{2} + \dots + a_{mn} x_{n} &= b_{m} \\
		\end{align}$$
		- Where $a_{ij} \in \mathbb{R}$ and $b_{i} \in \mathbb{R}$
		- This equation is general form of a system of linear equations
			- $x_{1}, \dots, x_{n}$ are unknowns of the system
			- Every $n$-tuple $(x_{1}, \dots, x_{n}) \in \mathbb{R}^n$ that satisfies these equations is a solution of the linear equation system
- **Example 2.2**
	- $$\begin{align}
	\begin{matrix}
	x_{1} &+ &x_{2} &+ &x_{3} &= &3 &(1)\\
	x_{1} &- &x_{2} &+ &2x_{3} &= &2 &(2)\\
	2x_{1} & & &+ &3x_{3} &= &1 &(3)
	\end{matrix}
	\end{align}$$
	- This system of equations has no solution
		- Adding $(1) + (2)$ we get $2x_{1} + 3x_{3} = 2$
		- However the third equation says $2x_{1} + 3x_{3} = 1$
		- Therefore this is a contradiction
	- $$\begin{align}
	\begin{matrix}
	x_{1} &+ &x_{2} &+ &x_{3} &= &3 &(1)\\
	x_{1} &- &x_{2} &+ &2x_{3} &= &2 &(2)\\
	& &x_{2} &+ &x_{3} &= &2 &(3)
	\end{matrix}
	\end{align}$$
	- This system of equations has a unique solution
		- From $(1) - (3)$ we get $x_{1} = 1$. 
		- Plugging into $(2)$ we get $1 - x_{2} + 2x_{3} = 2$, which we can rewrite as $-x_{2} + 2x_{3} = 1$. 
		- Adding this to $(3)$ we get $3x_{3} = 3$, therefore $x_{3} = 1$.
		- Substituting the values of $x_{1}$ and $x_{3}$ into $(1)$ we get $1 + x_{2} + 1 = 3$, which we can rewrite as $x_{2} = 1$
		- Therefore $(1, 1, 1)$ is the only possible and unique solution to this system of equations
	- $$\begin{align}
	\begin{matrix}
	x_{1} &+ &x_{2} &+ &x_{3} &= &3 &(1)\\
	x_{1} &- &x_{2} &+ &2x_{3} &= &2 &(2)\\
	2x_{1} & & &+ &3x_{3} &= &5 &(3)
	\end{matrix}
	\end{align}$$
	- This system of equations has infinite many solutions
		- Adding $(1) + (2)$ we get $2x_{1} + 3x_{3} = 5$
		- Notice that this is equivalent to the third equation