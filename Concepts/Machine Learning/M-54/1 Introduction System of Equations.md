# 1 Introduction System of Equations
# What is Linear Algebra?
- Most concrete form
	- Study of systems of equations like this one
$$\begin{align}
x + 2y + 3z = 6 \\
x - y + z = 1 \\
2x + 3y + 4z = 9
\end{align}$$
- Systems of equations have geometric meaning
	- Region where each equation is satisfied is a plane
	- Simultaneous solution to all equations is where the planes intersect
	- ![[Pasted image 20250328205815.png]]
- Linear algebra is basic tool for understanding computer graphics + vision
- Study of transformations of spaces which carry lines to lines
	- Lines get "transformed" into a place, but stays straight
	- What is a space, line, transformation?
- More abstract perspective is worth the effort
	- Many phenomena are linear in this abstract sense
	- Can all be studied using system of linear equations
- **Ex.** Schrodinger's equation for a quantum mechanical particle
	- ![[Pasted image 20250328210119.png]]
- Here, the "space" is a space of functions which might be our might be desired $\psi (\text{x}, t)$
	- Linear transformations are partial differential operators
- But this equation is still linear
	- By end of class, Schrodinger's equation should look no worse than regular system of equations
## Many phenomena are approximately linear
- Linear regression
	- Discovering statistical linearities plays role in experimental sciences
- ![[Pasted image 20250328210629.png]]
- Language for discussing differential and integral calculus, especially in higher dimensions
	- **Ex.** Getting the tangent of a function

# What will you learn?
- Concrete procedures
- Abstract notions about linearity
- Many real-world examples
# Linear equations
- **Ex.** Equation $x + 2y + 3z = 6$, linear in $x, y, z$
- Equation in variables $x_{1}, x_{2}, \dots, x_{n}$ is linear if it can be put in the form
	- $a_{1}x_{1} + a_{2}x_{2} + \dots + a_{n} x_{n} = b$
- Where coefficients and $b$ do not depend on any of $x_{i}$
	- Usually $a_{i}$ and $b$ will just be explicit real or complex numbers
- Nonexample
	- Equation $x^3 = 6$ is not linear
	- Might try writing as $(x^2)x = 6$, but now the coefficient depends on $x$
- Example-nonexample
	- Equation $xy = 1$ linear is $x$, and is linear in $y$
	- However this is not linear in $x$ and $y$
- Confusing example
	- $\dfrac{x_{1}}{x_{2}} = 4$
	- Equation is equivalent to equation $x_{1} = 4x_{2}$ if $x_{2} \neq 0$
	- However if $x_{2}$ can be 0, then the equation is not linear
# Systems of linear equations
- **DEFINE:** 
	- System of linear equations in $x_{1}, \dots, x_{n}$ is finite collections of linear equations in $x_{1}, x_{n}$
	- Helpful to "line up the x's" and write systems in form
		- ![[Pasted image 20250328211344.png]]
- This is a system of $m$ linear equations and $n$ unknowns
- **DEFINE:** 
	- Set of solutions to a system of linear equations in $x_{1}, .., x_{n}$ is set of all tuples of numbers $(s_{1}, \dots, s_{n})$ such that substituting $s_{i}$ for $x_{i}$ gives an identity
- **Ex.**
	- Equation $x_{1} = 5$ has solution set $\{ 5 \}$
	- System $x_{1} = 2$ and $x_{1 + x_{2} = 7}$ has solution set $\{ (2,5) \}$
	- System $x_{1} = 2$ and $x_{1} = 7$ has empty solution set
	- System $x_{1} + x_{2} = 0$ has solution set $\{ (s, -s) \}$ where $s$ takes any value
		- Note that this system has an infinite number of valid solutions
# Consistency and inconsistency
- **DEFINE:**
	- System of linear equations is consistent if it has solutions and inconsistent otherwise
	- Consistent if there are $1$ or $\infty$ solutions
	- Inconsistent if $0$ solutions
- We saw examples of both consistent and inconsistent systems
	- Either 0, 1, or $\infty$ solutions
	- Lines are either
		- Parallel (0 solution)
		- Intersect (single solution)
		- Lay on-top of each other (infinite solutions)
# Why did it work?
- Solving one linear equation for given variable and plugging it into another linear equation
	- Same as 
- Adding a multiple of first equation to the second
	- **NOTE:** Only true for linear equations
- We solved system of linear equations by transforming them into simpler, but equivalent systems of equations
- We only used "Elementary" equivalences
	- Adding multiple of one equation to another
	- Multiplying equation by a nonzero number
# Augmented matrix
- We can abbreviate system of equations
	- $$\begin{align} \\
		x + 2y + 3z = 6 \\
		x - y + z = 1 \\
		2x + 3y + 4z = 9
		\end{align}$$
- as
	- $$\left[
		\begin{array}{ccc|c}
		1 &2 &3 &6 \\
		1 &-1 &1 &1 \\
		2 &3 &4 &9
		\end{array}\right]$$
- **DEFINE:** Augment matrix of a sytem
	- As many rows as equation
	- One more column than number of unknowns
	- Last column is used for RHS of each equation
# Row reduction
- At each stage, reduced number of non-zero entries in matrix by adding multiples of one row to another
	- AKA Gaussian elimination, or row reduction
	- It was 9 chapters on mathematical art 2000 years before
$$\begin{align}
&\left[
\begin{array}{cc|c}
1 &s &0 \\
-1 &1 &s + 1 \\
\end{array}\right] \\
&= \left[
\begin{array}{cc|c}
0 &s &0 \\
0 &s + 1 &s + 1 \\
\end{array}\right] \text{1 + 2}
\end{align}$$