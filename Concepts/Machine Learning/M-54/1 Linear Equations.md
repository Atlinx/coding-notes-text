# 1 Linear Equations in Linear Algebra
# 1.1 System of Linear Equations
- **DEFINE:** Linear equation ^linear-equation
	- Linear equation of variables $x_{1}, \dots, x_{n}$ is an equation that can be written in form
		- $$a_{1} x_{1} + a_{2}x_{2} + \dots + a_{n} x_{n} = b$$
	- Coefficients -> $a_{1}, \dots, a_{n}$
	- Coefficients + $b$ are real or complex numbers
	- Subscript $n$ is a positive integer
- **DEFINE:** Linear system ^linear-system
	- Collection of one or more linear equations
	- **Ex.**
	- $$\begin{align}
		2x_{1} - x_{2} + 1.5x_{3} = 8 \\
		x_{1} - 4x_{3} = -7
		\end{align}$$
- **DEFINE:** Solution of a system ^solution-of-system
	- List of numbers $(s_{1}, s_{2}, \dots, s_{n})$ that make each equation in a system of equations true when substituted in for $x_{1}, x_{3}, \dots, x_{n}$
	- System of linear equations has either
		- No solution
		- Exactly one solution
		- Infinitely many solutions
	- Solutions can be visualized -> consider a system of 2 equations with two terms $x_{1}$, $x_{2}$^visualize-solutions
		- ![[Pasted image 20250329104843.png]]
			- If the two lines intersect, then there's one solution
		- ![[Pasted image 20250329104851.png]]
			- If two lines are parallel, then there are no solutions
			- If the two lines lie on top of each other, then there are infinitely many solutions
- **DEFINE:** Solution set ^solution-set
	- Set of all possible solutions to a linear system of equations
- **DEFINE:** Equivalence ^equvalent
	- Two linear systems of equivalent if they have same solution se
- **DEFINE:** Consistency ^consistency
	- System of linear equations is consistent if it has a solution (one solution or infinitely many solutions)
	- System of linear equations is inconsistent if it has no solutions
## Matrix Notation
- **DEFINE:** Matrix ^matrix
	- Recording a linear system of equations in a regular array
	- $$\begin{array}{}
		x_{1} &-2x_{2} &+x_{3} &= 0 \\
		&2x_{2} &-8x_{3} &= 8 \\
		5x_{1} & &-5x_{3} &= 10
		\end{array}$$
	- **DEFINE:** Coefficient matrix ^coefficient-matrix
		- Coefficients of each variable are aligned in columns
		- $$\begin{bmatrix}
			1 &-2 &1 \\
			0 &2 &-8 \\
			5 &0 &-5
			\end{bmatrix}
			$$
		- **NOTE:** Terms of $x_{i}$ that do not appear in an equation have a coefficient 0, since that causes the term to disappear
	- **DEFINE:** Augmented matrix ^augmented-matrix
		- This is the coefficient matrix with an extra column, that includes the $b$ part of the linear system of equations
		- This extra column is often separated by a line
		- $$\left[\begin{array}{ccc|c}
			1 &-2 &1 &0\\
			0 &2 &-8 &8\\
			5 &0 &-5 &10
			\end{array}\right]
			$$
- **DEFINE:** Size of matrix ^size-of-matrix
	- Number of rows and columns a matrix has
	- $m \times n$ matrix has $m$ rows and $n$ columns
		- **NOTE:** We always list the rows first when describing the size of a matrix
		- **Ex.** A matrix with 3 rows and 4 columns is known as a "3 x 4" matrix
## Solving a Linear System
- Idea -> replace one system of equations with an equivalent system
$$\begin{align}
\begin{array}{}
x_{1} &-2x_{2} &+x_{3} &= 0 \\
&2x_{2} &-8x_{3} &= 8 \\
5x_{1} & &-5x_{3} &= 10
\end{array} \\ \\
\left[\begin{array}{ccc|c}
1 &-2 &1 &0\\
0 &2 &-8 &8\\
5 &0 &-5 &10
\end{array}\right] \\
\left[\begin{array}{ccc|c}
1 &-2 &1 &0\\
0 &2 &-8 &8\\
0 &10 &-10 &10
\end{array}\right] &-5(1) + (2) \to (2) \\
\left[\begin{array}{ccc|c}
1 &0 &-7 &8\\
0 &2 &-8 &8\\
0 &10 &-10 &10
\end{array}\right] &(2) + (1) \to (1)\\
\left[\begin{array}{ccc|c}
1 &0 &-7 &8\\
0 &2 &-8 &8\\
0 &0 &30 &-30
\end{array}\right] &-5(2) + (3) \to (3)\\
\left[\begin{array}{ccc|c}
1 &0 &-7 &8\\
0 &2 &-8 &8\\
0 &0 &1 &-1
\end{array}\right] & \frac{1}{30}(3) \to (3)\\
\left[\begin{array}{ccc|c}
1 &0 &-7 &8\\
0 &2 &-8 &8\\
0 &0 &1 &-1
\end{array}\right] & 8(3) + (2) \to (2)\\
\left[\begin{array}{ccc|c}
1 &0 &-7 &8\\
0 &2 &0 &0\\
0 &0 &1 &-1
\end{array}\right] & 8(3) + (2) \to (2)\\
\left[\begin{array}{ccc|c}
1 &0 &0 &1\\
0 &2 &0 &0\\
0 &0 &1 &-1
\end{array}\right] & 7(3) + (1) \to (1)\\
\left[\begin{array}{ccc|c}
1 &0 &0 &1\\
0 &1 &0 &0\\
0 &0 &1 &-1
\end{array}\right] & 1(2) \to (2)
\end{align}$$
- Each 3-term equation in system of equations above determines a plane in three-dimensional space, similar to [[#^visualize-solutions|we can visualize 2-term systems as lines]]
	- The point which intersects all three points is the solution
	- ![[Pasted image 20250329102955.png]]
- **DEFINE:** Elementary row operations ^elementary-row-operations
	- Operations that can be done on rows of a matrix that produce an equivalent equation
	- Types
		1. Replacement -> replace one row by sum of itself and a multiple of another row
			- My notation: $N(a) + (b) \to (b)$
		2. Interchange -> interchange (swap) two rows
			- My notation: $(a) \leftrightarrow (b)$
		3. Scaling -> multiple all entries in a row by a nonzero constant
			- My notation: $N(a) \to (a)$
	- Notation legend
		- $a,b =$ are distinct row numbers
		- $N =$ some non-zero constant
	- Row operations are reversible
- **DEFINE:** Row equivalence
	- Two matrices are **row equivalent** if there is a sequence of elementary row operations that transforms one matrix into another
- If augmented matrices of two linear systems are row-equivalent, then two systems have same solution set
## Existence and Uniqueness Questions
- Two fundamental questions about linear system
	- Is the system consistent ($\geq$ 1 solution)?
	- If solution exists, is the solution unique (exactly $1$ solution)?
- **DEFINE:** Triangular form ^triangular-form
	- An augmented matrix where every row has zeroes before a one, and each row has one or more zeroes than the previous row
	- This form a "triangle of zeroes" on the bottom left of the matrix
	- $$\begin{align}
		\left[\begin{array}{ccc|c}
		1 &0 &-7 &8\\
		0 &1 &-4 &4\\
		0 &0 &1 &-1
		\end{array}\right] \\
		\left[\begin{array}{cccc|c}
		0& 1 &4 &-2 &3\\
		0& 0 &1 &-3 &3\\
		0& 0 &0 &1 &-2
		\end{array}\right] \\
		\left[\begin{array}{cccc|c}
		1& 0 &3 &-7 &3\\
		0& 1 &2 &-4 &7\\
		0& 0 &0 &1 &-1
		\end{array}\right] \\
		\end{align}$$
# 1.2 Row Reduction and Echelon Forms
- **DEFINE:** Leading entry ^leading-entry
	- The leading entry of a row is the left-most non-zero entry in the row
	- $$\begin{bmatrix}
		0& 1 &4 &-2 &3\\
		0& 0 &7 &-3 &3\\
		0& 0 &0 &5 &-2
		\end{bmatrix}$$
		- **Ex.** Leading entry for first row = 1, leading entry for second row = 7, leading entry for third row = 5
- **DEFINE:** Row echelon form ^row-echelon-form
	- AKA echelon form
	- A regular matrix is in echelon form if it has three properties
		1. All nonzero rows are above any rows of all zeroes
		2. Each leading entry of row is in a column to right of leading entry of the row above it
		3. All entries in a column below a leading entry are zeroes
	- Leading entries follow a "step-like" pattern moving down and to the right
	- $$\begin{bmatrix}
		0& 1 &4 &-2 &3\\
		0& 0 &7 &-3 &3\\
		0& 0 &0 &5 &-2
		\end{bmatrix}$$
	- ![[Pasted image 20250329111135.png]]
	- If matrix $A$ is row equivalent to an echelon matrix $U$, then
		- $U$ is a row echelon form of $A$
	- Any matrix can be row-reduced (transformed using elementary row operations) into more than one possible echelon forms, using different sequences of operations
- **DEFINE:** Reduced row echelon form ^redued-row-echelon-form
	- AKA reduced echelon form
	- A matrix in echelon form that also has the following properties
		1. Leading entry in each nonzero row is 1
		2. Each leading 1 is only nonzero entry in its column
	- $$\begin{bmatrix}
		0& 1 &4 &-2 &3\\
		0& 0 &1 &-3 &3\\
		0& 0 &0 &0 &-2
		\end{bmatrix}$$
	- ![[Pasted image 20250329111149.png]]
	- If matrix $A$ is row equivalent to a reduced echelon matrix $U$, then
		- $U$ is a reduced row echelon form of $A$
	- Each matrix is row-equivalent to **one and only one** reduced echelon matrix
## Pivot Positions
- **DEFINE:** Pivot position
	- Pivot position of a matrix $A$ is a location in $A$ that corresponds to a leading $1$ in the reduced echelon form of $A$
	- **DEFINE:** Pivot column
		- Column of $A$ that contains a pivot position
	- ![[Pasted image 20250329111740.png]]
- **DEFINE:** Pivot
	- A nonzero number in a pivot position that is used to created zeroes below itself using row operations
## The Row Reduction Algorithm
- **DEFINE:** Row reduction algorithm
	- Takes a matrix and converts it into an echelon form
	- The fifth step produces a matrix in reduced echelon form
	- Steps
		1. Begin with leftmost nonzero column. This is a pivot column, and the pivot position is at the top of this column.
		2. Select nonzero entry in the pivot column as a pivot
			1. If necessary, interchange rows to move a nonzero entry into the pivot position
		3. Use row replacement operations to create zeroes in all positions below the pivot
		4. Cover (ignore) the row containing the pivot position and any above it
			1. Repeat steps 1-3 to the submatrix that remains (the rows below)
		5. Begin with rightmost pivot and work upward to the left, creating zeroes above each pivot
			1. If a pivot is not 1, make it 1 by a scaling operation
	- Steps 1-4 -> **forward phase** of the row reduction algorithm
	- Step 5 -> **backward phase** of the row reduction algorithm
	- **DEFINE:** Partial pivoting
		- Computer programs usually as a pivot the entry in the column with the largest absolute value
		- Reduces roundoff errors in calculations
## Solutions of Linear Systems
- Consider a system of equations that has been converted into the following reduced echelon form
	- $$\begin{align}
		\begin{array}{}
		x_{1} & &-5x_{3} &= 1 \\
		&x_{2} &+x_{3} &=4 \\
		& &0 &= 0
		\end{array} \\ \\
		\begin{bmatrix}
		1 &0 &-5 &1 \\
		0 &1 &1 &4 \\
		0 &0 &0 &0
		\end{bmatrix}
		\end{align}$$
- Variables $x_{1}, x_{2}$ corresponding to pivot columns are called **basic variables**
- $x_{3}$ is called a **free variable**
- Solution set of system can be described as solving system of equations for basic variables in terms of free variables
	- Reduced row echelon form places each basic variable in one equation (one row)
	- We can then solve for each basic variable in each equation (each row)
	- $$\begin{cases}
		x_{1} = 1 + 5x_{3} \\
		x_{2} = 4 - x_{3} \\
		x_{3} \text{ is free}
		\end{cases}$$
- **DEFINE:** Basic variables ^basic-variables
	- Variables that are a pivot in the reduced-echelon form of the matrix
	- Appears in exactly one matrix
- **DEFINE:** Free variables ^free-variables
	- Variables that are not a pivot in the reduced-echelon form of the matrix
	- May occur in multiple equations in the reduced-echelon form of the matrix
	- We are free to choose any value of the free variables, and then solve for basic variables to identify a solution to the system
		- Every different choice of free variables determines a different solution of the system
		- Every solution of the system is determined by a choice of free variables
## Parametric Descriptions of Solution Sets
- **DEFINE:** Parametric equation/description
	- Something that expresses several quantities (like coordinates of a point) as functions of one or several parameters
	- **Ex.** Parametric equation for a circle, that's drawn over time using $t$
		- $$\begin{cases}
			x = \cos(t) \\
			y = \sin(t) \\
			\end{cases}$$
		- Quantities/outputs are $x$ and $y$, which represent a 2D coordinate
		- The parameter is $t$, which represents time
- **DEFINE:** Parametric descriptions of solution sets
	- A representation of solution sets as a parametric equation where
		- The parameters are **free variables**
		- The outputs are the **basic variables**
	- $$\begin{cases}
		x_{1} = 1 + 5x_{3} \\
		x_{2} = 4 - x_{3} \\
		x_{3} \text{ is free}
		\end{cases}$$
	- When system is consistent and has free variables, there are many parametric descriptions of the system
	- When system is inconsistent, the solution set has no parametric representation
## Back-Substitution
$$\begin{array}{}
x_{1} &- 7x_{2} &+ 2x_{3} &- 5x_{4} &+ 8x_{5} &= 10 \\
&x_{2} &- 3x_{3} &+ 3x_{4} &+ x_{5} &= -5 \\
&&&x_{4} &- x_{5} &= 4
\end{array}$$
- Back-substitution
	- Using equation 3 to solve for $x_{4}$ in terms of $-x_{5}$, and then substitute it into equation 2
- Backward-phase of row-reduction algorithm has same number of steps, but reduces likelihood of errors during hand-computations
## Existence and Uniqueness Questions
- **DEFINE:** Existence and uniqueness theorem
	- Linear system is consistent if and only if rightmost column of augmented matrix is not a pivot column
		- That is, if and only if echelon form of augmented matrix has no row in the form of
			- $\left[\begin{array}{ccc|c}0 &\dots &0 &b\end{array}\right]$ where $b$ is nonzero
	- If a linear system is consistent, the solution set either contains
		- A unique solution, when there are no free variables
		- Infinitely many solutions, when there is at least one free variable
- Solving linear system using row reduction
	1. write augmented matrix of system
	2. Use row-reduction algorithm to obtain equivalent augmented matrix in echelon form. Decide whether system is consistent. If there is no solution, stop.
	3. Continue row reduction to obtain reduced echelon form
	4. Write system of equations corresponding to matrix from step 3
	5. Rewrite each nonzero equation so its basic variable is expressed in terms of any free variables in equation
# 1.3 Vector Equations
## Vectors in $\mathbb{R}^2$
- **DEFINE:** Vector ^vector
	- Ordered list of numbers
	- Column vector -> matrix with only one column
	- Row vector -> matrix with only row column
	- **Ex.** Column vectors with 2 entries
		- $$\mathbf{u} = \begin{bmatrix}3 \\-1\end{bmatrix} \hspace{1em} \mathbf{v} = \begin{bmatrix}.2 \\.3\end{bmatrix} \hspace{1em} \mathbf{w} = \begin{bmatrix}w_{1} \\w_{2}\end{bmatrix}$$
		- Where $w_{1}$ and $w_{2}$ are real numbers
		- Set of all vectors with two entries is denoted as $\mathbb{R}^2$
			- Two vectors in $\mathbb{R}^2$ are equal if their corresponding entries are equal
			- $$\begin{bmatrix}4 \\ 7\end{bmatrix} \neq \begin{bmatrix}7 \\ 4\end{bmatrix}$$
				- Because vectors in $\mathbb{R}^2$ are ordered pairs of real numbers
- Sum of vectors is adding their entries
	- $$$$
- **DEFINE:** Scalar ^scalar
	- A regular number (not a vector)
	- **Ex.** $3$ and $-4.3$ are scalars
- Given two vectors $\mathbf{u}$ and $\mathbf{v}$, their sum is vector $\mathbf{u} + \mathbf{v}$ obtained by adding corresponding entries of $\mathbf{u}$ and $\mathbf{v}$ together 
	- Given
		- $\mathbf{u} = \begin{bmatrix}1 \\ -2\end{bmatrix}$
		- $\mathbf{v} = \begin{bmatrix}2 \\ 5\end{bmatrix}$
	- $\mathbf{u} + \mathbf{v} = \begin{bmatrix}1 \\ -2\end{bmatrix} + \begin{bmatrix}2 \\ 5\end{bmatrix} = \begin{bmatrix}1 + 2 \\-2 + 5\end{bmatrix} = \begin{bmatrix}3 \\ 3\end{bmatrix}$
- Scalar multiple of $\mathbf{u}$ and a scalar $c$
	- Given
		- $\mathbf{u} =\begin{bmatrix}3 \\ -1\end{bmatrix}$
		- $c = 5$
	- $c \mathbf{u} = 5\begin{bmatrix}3 \\ -1\end{bmatrix} = \begin{bmatrix}15 \\ -5\end{bmatrix}$
## Geometric Descriptions of $\mathbb{R}^2$
- We can interpret vectors in $\mathbb{R}^2$ as coordinates on a 2D plane
	- Geometric point $(a, b)$ corresponds to the column vector $\begin{bmatrix}a \\ b\end{bmatrix}$
	- $\mathbb{R}^2$ therefore is the set of all points on the plane
	- ![[Pasted image 20250329120802.png]]
		- Points are often visualized with an arrow drawn from the origin to the point
- **DEFINE:** Parallelogram rule for addition ^parallelogram-rule-addition
	- If $\mathbf{u}$ and $\mathbf{v}$ in $\mathbb{R}^2$ are points in the plane, then $\mathbf{u} + \mathbf{v}$ corresponds to fourth vertex on parallelogram
	- ![[Pasted image 20250329120735.png]]
		- You can imagine this as taking the first vector $\mathbf{u}$ and laying it out on the origin
		- Then take the second vector $\mathbf{v}$ and place it such that its starting point is on the end of vector $\mathbf{u}$
		- Then the resulting vector from the origin to the tip of the second vector $\mathbf{v}$ is the result of the vector addition
## Vectors in $\mathbb{R}^3$
- Vectors in $\mathbb{R}^3$ are $3 \times 1$ column matrices, and can be represented in a 3D coordinate space
	- $\mathbf{a} = \begin{bmatrix}2 \\ 3 \\ 4\end{bmatrix}$
	- ![[Pasted image 20250329120935.png]]
## Vectors in $\mathbb{R}^n$
- If n is a positive integers, then $\mathbb{R}^n$ is collection of all lists of $n$ real numbers, usually written as $n \times 1$ column matrices
	- $\mathbf{u} = \begin{bmatrix}u_{1} \\ u_{2} \\ \vdots \\ u_{n}\end{bmatrix}$
- **DEFINE:** Zero vector ^zero-vector
	- Vectors whose entries are all zero, denoted by $\mathbf{0}$
	- **Ex.**
		- $\mathbf{0} = \begin{bmatrix}0 \\ 0 \\ \vdots \\ 0\end{bmatrix}$
- Scalar multiplication and vector addition are defined entry by entry
- **DEFINE:** Algebraic properties of $\mathbb{R}^n$ ^properties-of-Rn
	- For all vectors $\mathbf{u}, \mathbf{v}, \mathbf{w}$ in $\mathbb{R}^n$ and all scalars $c, d$
		1. $\mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u}$
		2. $(\mathbf{u} + \mathbf{v}) + \mathbf{w} = \mathbf{u} + (\mathbf{v} +\mathbf{w})$
		3. $\mathbf{u} + \mathbf{0} = \mathbf{0} + \mathbf{u} = \mathbf{0}$
		4. $\mathbf{u} + (-\mathbf{u}) = \mathbf{-u} + \mathbf{u} = \mathbf{0}$
			1. $\mathbf{-u}$ denotes $(-1)\mathbf{u}$
		5. $c(\mathbf{u} + \mathbf{v}) = c \mathbf{u} + c \mathbf{v}$
		6. $(c + d)\mathbf{u} = c \mathbf{u} + d \mathbf{u}$
		7. $c(d\mathbf{u}) = (cd)\mathbf{u}$
		8. $1 \mathbf{u} = \mathbf{u}$
## Linear Combinations
- **DEFINE:** Linear combination
	- Given vectors $\mathbf{v}_{1}, \mathbf{v}_{2}, \dots \mathbf{v}_{p}$ in $\mathbb{R}^n$, and given scalars $c_{1}, c_{2},\dots c_{p}$
		- $\mathbf{y} = c_{1} \mathbf{v_{1}} + \dots + c_{p} \mathbf{v}_{p}$
	- $\mathbf{y}$ is called the linear combination of $\mathbf{v}_{1}, \dots, \mathbf{v}_{p}$ with weights $c_{1}, \dots, c_{p}$
	- Weights in a linear combination can be any real number, including zero
	- **Ex.**
		- $\mathbf{0} = 0\mathbf{v}_{1} + 0\mathbf{v}_{2}$
		- $\frac{1}{2}\mathbf{v}_{1} = \frac{1}{2} \mathbf{v}_{1} + 0 \mathbf{v}_{2}$
- Determining if a vector $\mathbf{b}$ can be written as a linear combination of a list of vectors $\mathbf{a}_{1}, \mathbf{a}_{2}, \dots \mathbf{a}_{n}$
	- A vector equation
		- $x_{1} \mathbf{a}_{1} + x_{2} \mathbf{a}_{2} + \dots + x_{n} \mathbf{a}_{n} = \mathbf{b}$
	- Has the same solution set as linear system whose augmented matrix is
		- $\left[\begin{array}{cccc|c}\mathbf{a}_{1} &\mathbf{a}_{2} &\dots &\mathbf{a}_{n} &\mathbf{b}\end{array}\right]$
	- $\mathbf{b}$ can be generated by a linear combination of $\mathbf{a}_{1}, \dots, \mathbf{a}_{n}$ if and only if there exists a solution to the linear system corresponding to the augmented matrix above
- **DEFINE:** Span ^span
	- Set of all vectors that can be generated or written as a linear combination of a fixed set of vectors $\{ \mathbf{v}_{1}, \dots \mathbf{v}_{p} \}$
	- Denoted by $\text{Span} \{ \mathbf{v_{1}, \dots, \mathbf{v_{p}}} \}$
		- Represents collection of vectors that can be rewritten in the form
			- $c_{1} + \mathbf{v}_{1} + c_{2} \mathbf{v}_{2} + \dots + c_{p} \mathbf{v}_{p}$
		- With $c_{1}, \dots, c_{p}$ scalars
	- AKA subset of $\mathbb{R}^n$ spanned by $\mathbf{v}_{1}, \dots \mathbf{v}_{p}$
	- Span can be represented as line using 1 vector, a plane using 2 vectors, a cube using 3 vectors, or higher dimensional shapes
## Geometric Description of $\text{Span}\{ \mathbf{v} \}$ and $\text{Span}\{ \mathbf{u}, \mathbf{v} \}$
- Span of a a single vector is a line
- Span of two vectors (as long as they aren't multiples of each other) is a plane
- ![[Pasted image 20250329123107.png]]
## Linear Combinations in Applications
- Linear combinations arise when a quantity such as "cost" is broken down into several categories
- **Ex.** Cost of producing several units of an item when the cost per unit is known
	- Company manufactures two products
		- For 1 unit of product B, company spends $.45 on materials, $.25 on labor, and $.15 on overhead
		- For 1 unit of product C, company spends $.4 on materials, $.3 on labor, and $.15 on overhead
		- $\mathbf{b} = \begin{bmatrix}.45 \\ .25 \\ .15\end{bmatrix}$
		- $\mathbf{c} = \begin{bmatrix}.4 \\ .3 \\ .15\end{bmatrix}$
		- $\mathbf{b}$ and $\mathbf{c}$ represent "costs per unit" for the two products
			- $100\mathbf{b} =$ costs for producing 100 units of product $\mathbf{b}$
# 1.4 Matrix Equation $A\mathbf{x} = \mathbf{b}$
- Linear combination of vectors can be represented as the product of a matrix and a vector
	- Given $A$ is an $m \times n$ matrix with columns $\mathbf{a}_{1}, \dots, \mathbf{a}_{n}$, and if $\mathbf{x}$ is in $\mathbb{R}^n$
	- Product of $A$ and $\mathbf{x}$ is denoted as $A\mathbf{x}$
		- This product is the linear combination of the columns of $A$ using corresponding entries in $\mathbf{x}$ as weights
		- $$A \mathbf{x} = \begin{bmatrix} \mathbf{a}_{1} &\mathbf{a}_{2} &\dots &\mathbf{a}_{n}\end{bmatrix} \begin{bmatrix} x_{1} \\ \vdots \\ x_{n}\end{bmatrix} = x_{1} \mathbf{a}_{1} + x_{2} \mathbf{a}_{2} + \dots + x_{n} \mathbf{a}_{n}$$
		- **Ex.** $\begin{bmatrix}1 &2 &-1 \\0 &-5 &3\end{bmatrix} \begin{bmatrix}4 \\3 \\7\end{bmatrix} = 4 \begin{bmatrix}1 \\0\end{bmatrix} + 3 \begin{bmatrix}2 \\ -5\end{bmatrix} + 7 \begin{bmatrix}-1 \\ 3\end{bmatrix} = \begin{bmatrix}3 \\ 6\end{bmatrix}$
- **DEFINE:** Matrix equation
	- Given $A$ is an $m \times n$ matrix with columns $\mathbf{a}_{1}, \dots, \mathbf{a}_{n}$, and if $\mathbf{b}$ is in $\mathbb{R}^n$, the matrix equation
		- $A \mathbf{x} = \mathbf{b}$
	- has the same solution set as the vector equation
		- $x_{1} \mathbf{a}_{1} + \dots + x_{n} \mathbf{a}_{n} = \mathbf{b}$
	- which has same solution se as system of linear equations whose augmented matrix is
		- $\left[ \begin{array}{cccc|c} \mathbf{a}_{1} &\mathbf{a}_{2} &\dots &\mathbf{a}_{n} &\mathbf{b} \end{array} \right]$
## Existence of Solutions
- Equation $A \mathbf{x} = \mathbf{b}$ has a solution if and only if $\mathbf{b}$ is a linear combination of the columns of $A$
- Theorem for a coefficient matrix
	- Let $A$ be an $m \times n$ matrix. Following statements are logically equivalent. For a particular $A$, they are either all true or all false
	- For each $\mathbf{b}$ in $\mathbb{R}^m$ the equation $A\mathbf{x} = \mathbf{b}$ has a solution
	- Each $\mathbf{b}$ in $\mathbb{R}^m$ is a linear combination of columns of $A$
	- The columns of $A$ span $\mathbb{R}^m$
	- $A$ has a pivot position in every row
## Computation of $A \mathbf{x}$
- **DEFINE:** Dot product ^dot-product
	- Dot product of two vectors $\mathbf{a}$ and $\mathbf{b}$ is
	- $\mathbf{a} \cdot \mathbf{b} = a_{1} b_{1} + a_{2} b_{2} + \dots + a_{n} b_{n}$ 
- Steps to compute $A\mathbf{x}$
	- Go row by row in the matrix, taking each entry of row multiplied by the corresponding entry in the vector $\mathbf{x}$
- **DEFINE:** Identity matrix ^identity-matrix
	- An $n \times n$ matrix where diagonal of the matrix are all ones, and the remaining entries are all zeroes
	- **Ex.** Identity matrix of $\mathbb{R}^3$
		- $I_{3} = \begin{bmatrix}1 &0 &0 \\ 0 &1 &0 \\ 0 &0 &1\end{bmatrix}$
	- Property of identity matrix
		- $I \mathbf{x} = \mathbf{x}$
		- Same idea as $1 \times 3 = 3$ in regular arithmetic
## Properties of Matrix-Vector Product $A\mathbf{x}$
- If $A$ is $m \times n$ matrix, $\mathbf{u}$ and $\mathbf{v}$ are vectors in $\mathbb{R}^n$, and $c$ is scalar, then
	- $A(\mathbf{u} + \mathbf{v}) = A\mathbf{u} + A\mathbf{v}$
	- $A(c \mathbf{u}) = c(A\mathbf{u})$
# 1.5 Solution Sets of Linear Systems
## Homogenous Linear Systems
- **DEFINE:** Homogenous ^homogenous
	- A system of linear equations is homogenous if it can be written in form $A \mathbf{x} = \mathbf{0}$ where $A$ is $m \times n$ matrix and $\mathbf{0}$ is the zero vector in $\mathbb{R}^m$
	- Trivial solution -> $A \mathbf{x} = \mathbf{0}$ always has at least one solution $\mathbf{x} = \mathbf{0}$
	- Nontrivial solution -> nonzero vector $\mathbf{x}$ that satisfies $A\mathbf{x} = \mathbf{0}$
- Homogenous equation has nontrivial solution if and only if the equation has at least one free variable
	- Idea -> Are there useless vectors in the matrix that don't meaningfully contribute to the span?
	- These useless vectors will appear as free variables, because the column they take up will be useless
## Parametric Vector Form
- **DEFINE:** Parametric vector equation
	- $\mathbf{x} = t_{1} \mathbf{v_{1}} + t_{2} \mathbf{v}_{2} + \dots + t_{n} \mathbf{v}_{n}$ where $\mathbf{v}_{i}$ is in $\mathbb{R}$
- **DEFINE:** Parametric vector form
	- A solution set of a system of linear equations is in parametric vector form if its written as a parametric vector equation
	- **Ex.** Solutions for $10x_{1} - 3x_{2} - 2x_{3} = 0$
		- Augmented matrix = $\left[\begin{array}{ccc|c}10 &-3 &-2 &0\end{array}\right]$
		- Solution is $x_{1} = .3x_{2} + .2x_{3}$
		- We can rewrite this as
		- $\mathbf{x} = \begin{bmatrix}x_{1} \\ x_{2} \\ x_{3}\end{bmatrix} = \begin{bmatrix}.3x_{2} + .2x_{3} \\ x_{2} \\ x_{3} \end{bmatrix} = \begin{bmatrix}.3x_{2} \\ x_{2} \\ 0\end{bmatrix} + \begin{bmatrix}.2 x_{3} \\ 0 \\ x_{3}\end{bmatrix} = x_{2} \begin{bmatrix}.3 \\ 1 \\ 0\end{bmatrix} + x_{3}\begin{bmatrix}.2 \\ 0 \\ 1\end{bmatrix}$
			- This is known as "decomposing" $\mathbf{x}$ into a linear combination of vectors, using the free variables $x_{2}, x_{3}$ as parameters
		- **NOTE:** 
			- The basic variables equal the equation we found with respect to the free variables
			- The free variables equal themselves
			- Afterwards we can separate out the vector into the $x_{1}, x_{2}$ and $x_{3}$ components
	- The solution to the system of linear equations can therefore be seen as a linear combination of vectors
## Solutions of Nonhomogeneous Systems
- Nonhomogeneous systems have $\mathbf{b} \neq \mathbf{0}$ 
- In this case the parametric vector from of the solutions contains an extra vector
- **Ex.** Solutions for $10x_{1} - 3x_{2} - 2x_{3} = 3$
	- Augmented matrix = $\left[\begin{array}{ccc|c}10 &-3 &-2 &3\end{array}\right]$
	- Solution is $x_{1} = .3x_{2} + .2x_{3} - .3$
	- $$\begin{align}
		\mathbf{x} &= \begin{bmatrix}x_{1} \\ x_{2} \\ x_{3}\end{bmatrix} = \begin{bmatrix}-.3 + .3x_{2} + .2x_{3} \\ x_{2} \\ x_{3} \end{bmatrix}  \\
		&= \begin{bmatrix}-.3 \\ 0 \\ 0\end{bmatrix} + \begin{bmatrix}.3x_{2} \\ x_{2} \\ 0\end{bmatrix} + \begin{bmatrix}.2 x_{3} \\ 0 \\ x_{3}\end{bmatrix}  \\
		&= \underbrace{ \begin{bmatrix}-.3 \\ 0 \\ 0\end{bmatrix} }_{ \text{extra!} } + x_{2} \begin{bmatrix}.3 \\ 1 \\ 0\end{bmatrix} + x_{3}\begin{bmatrix}.2 \\ 0 \\ 1\end{bmatrix}
		\end{align}$$
- Nonhomogeneous systems can be written in form
	- $\mathbf{x} = \mathbf{p} + t_{1} \mathbf{v}_{1} + \dots + t_{n} \mathbf{v}_{n}$
	- **Ex.** From the previous example above
		- $\mathbf{x} = \underbrace{ \begin{bmatrix}-.3 \\ 0 \\ 0\end{bmatrix} }_{ \large \mathbf{p} } + x_{2} \underbrace{ \begin{bmatrix}.3 \\ 1 \\ 0\end{bmatrix} + x_{3}\begin{bmatrix}.2 \\ 0 \\ 1\end{bmatrix} }_{ \large t \mathbf{v} }$
	- We can also frame the problem as solving for solutions of a homogenous system of equations, and then adding $\mathbf{p}$ (version of $\mathbf{b}$ in the reduced row-echelon form of the matrix) to it
		- $\mathbf{w} = \mathbf{p} + \mathbf{v}_{h}$, where $\mathbf{v}_{h}$ is any solution of homogenous equation $A\mathbf{x} = \mathbf{0}$
# 1.7 Linear Independence
- **DEFINE:** Linear independence
	- Set of vectors $\{ \mathbf{v}_{1}, \dots \mathbf{v}_{p} \}$ is **linearly independent** if the vector equation
		- $x_{1} \mathbf{v}_{1} + x_{2} \mathbf{v}_{2} + \dots + x_{p} \mathbf{v}_{p} = \mathbf{0}$ has only the trivial solution
	- Set of vectors $\{ \mathbf{v}_{1}, \dots \mathbf{v}_{p} \}$ is **linearly dependent** if there exists weights $c_{1}, \dots, c_{p}$ that are not all zero, such that
		- $c_{1} \mathbf{v}_{1} + c_{2} \mathbf{v}_{2} + \dots + c_{p} \mathbf{v}_{p} = \mathbf{0}$
		- A set of vectors that is linearly dependent means there are some vectors that don't meaningfully contribute to the span of the entire set
			- These vectors lie in the span of the other vectors
## Linear Dependence of Matrix Columns
- Given matrix $A = \begin{bmatrix}\mathbf{a}_{1} &\mathbf{a}_{2} &\dots &\mathbf{a}_{p}\end{bmatrix}$
	- Matrix equation $A\mathbf{x} = \mathbf{0}$
		- $x_{1} \mathbf{a}_{1} + x_{2} \mathbf{a}_{2} + \dots + x_{p} \mathbf{a}_{p} = \mathbf{0}$
	- The columns of the matrix are linearly independent if and only if the equation $A\mathbf{x} = \mathbf{0}$ has only the trivial solution
## Sets of One or Two Vectors
- Set containing only one vector $\mathbf{v}$ is linearly independent if and only if $\mathbf{v} \neq \mathbf{0}$
- The zero vector $\mathbf{0}$  is linearly dependent because $x_{1} \mathbf{0} = \mathbf{0}$ has many nontrivial solutions
	- Any set of vectors that contains the zero vector is linearly dependent
- If there are more vectors than there are entries in each vector, then the set is linearly dependent
	- That is, any set $\{ \mathbf{v}_{1}, \dots, \mathbf{v}_{p} \}$ in $\mathbb{R}^n$ is linearly dependent if $p > n$
	- This is because we're guaranteed to have free variables when we convert the matrix representation of the vectors to reduced echelon form
	- Also this is because you need exactly $n$ vectors to span the dimension $\mathbb{R}^n$
		- If we get more vectors than the dimension of the vectors themselves, then we have extra vectors that don't contribute meaningfully to the span of the set of vectors
- Set of two vectors is linearly dependent if $\geq$ one vector is a multiple of the other
# 1.8 Intro to Linear Transformations
- We can think of matrix $A$ as an object that "acts" on a vector $\mathbf{x}$ by multiplication to produce a new vector $A \mathbf{x}$
- **DEFINE:** Transformation
	- Transformation from $\mathbb{R}^n$ to $\mathbb{R}^m$ assigns each vector $\mathbf{x}$ in $\mathbb{R}^n$ to a vector $T(\mathbf{x})$ in $\mathbb{R}^m$
	- Domain -> Set of $\mathbb{R}^n$
		- Set of all inputs for $T(\mathbf{x})$, the different values that $\mathbf{x}$ can take on
	- Codomain -> Set of $\mathbb{R}^m$
		- Set of all possible values that $T(\mathbf{x})$ acn be 
	- For a vector $\mathbf{x}$, the vector $T(\mathbf{x})$ is called the **image** of x, under the action of $T$
	- Range -> The set of all images $T(\mathbf{x})$, the set of all outputs from $T(\mathbf{x})$
		- **NOTE:** Range is DIFFERENT from the codomain
			- The range is a subset of the codomain ($\text{range} \subseteq \text{codomain}$)
			- Range is the actual set of outputs that $T(\mathbf{x})$ output
	- ![[Pasted image 20250329135124.png]]
## Linear Transformation
- **DEFINE:** Linear transformation
	- Transformation $T$ is linear if
		- $T(\mathbf{u} + \mathbf{v}) = T(\mathbf{u}) + T(\mathbf{v})$ for all $\mathbf{u}, \mathbf{v}$ in domain of $T$
		- $T(c \mathbf{u}) = c T(\mathbf{u})$ for all scalars $c$ and all $\mathbf{u}$ in domain of $T$
	- Linear transformations preserve vector addition and scalar multiplication
- Specific types of transformations
	- Contraction/dilation
		- $T(\mathbf{x}) = r \mathbf{x}$
		- ![[Pasted image 20250329135615.png]]
	- Rotation
		- ![[Pasted image 20250329135731.png]]
# 1.9 The Matrix of Linear Transformation
- Every linear transformation is actually a matrix transformation $\mathbf{x} \mapsto A \mathbf{x}$
- A linear transformation is determined by what it does to the columns of an $n \times n$ identity matrix $I_{n}$
	- Let $T : \mathbb{R}^n \to \mathbb{R}^m$ be a linear transformation
		- There exists a unique matrix such that
			- $T(\mathbf{x}) = A\mathbf{x}$ for all $\mathbf{x}$ in $\mathbb{R}^n$
		- $A$ is an $m \times n$ matrix whose $j^{th}$ column is the vector $T(\mathbf{e}_{j})$, where $\mathbf{e}_{j}$ is the $j^{th}$ column of the identity matrix in $\mathbb{R}^n$
		- $A = \begin{bmatrix}T(\mathbf{e}_{1}) &\dots & T(\mathbf{e}_{n})\end{bmatrix}$
			- This is known as **the standard matrix for the linear transformation**
- Linear transformation -> focuses on a mapping
- Matrix transformation -> describes how a mapping is implemented
- The matrix corresponding to a linear transformation is called the **standard matrix for the linear transformation**^standard-matrix
## Geometric Linear Transformations of $\mathbb{R}^2$
## Existence and Uniqueness Questions
- **DEFINE:** Onto (surjective) mapping
	- A mapping $T : \mathbb{R}^n \to \mathbb{R}^m$ is **onto** $R^m$ if each $\mathbf{b}$ in $\mathbb{R}^m$ is the image of at least one $\mathbf{x}$ in $\mathbb{R}^n$
	- There exists at least one solution of $T(\mathbf{x}) = \mathbf{b}$
		- Every $\mathbf{b}$ has a solution $\mathbf{x}$
		- Multiple $\mathbf{x}$ can map to the same $\mathbf{b}$
		- The range = the codomain
	- It is not onto if there exists some $\mathbf{b}$ in $\mathbb{R}^m$ which $T(\mathbf{x}) = \mathbf{b}$ has no solution
	- ![[Pasted image 20250329151624.png]]
- **DEFINE:** One-to-one (injective)
	- A mapping $T: \mathbb{R}^n \to \mathbb{R}^m$ is one-to-one if each $\mathbf{b}$ in $\mathbb{R}^m$ is the image of at most one $\mathbf{x}$ in $\mathbb{R}^n$
	- For each $\mathbf{b}$ in $\mathbb{R}^m$, $T(\mathbf{x}) = \mathbf{b}$ has either a unique solution or no solution at all
		- Every $\mathbf{b}$ within the image/range of the transformation 
		- The range $\subseteq$ the codomain
			- There are no constraints on the codomain -> it may be equal to or be larger than the range of the transformation
	- ![[Pasted image 20250329151645.png]]
- **DEFINE:** Bijective mapping
	- A bijective mapping $T : \mathbb{R}^n \to \mathbb{R}^m$ is bijective if it is both onto and one-to-one
	- Every $\mathbf{x}$ maps to a unique $\mathbf{b}$ in the codomain $\mathbb{R}^m$
	- Every $\mathbf{b}$ has a solution $\mathbf{x}$
	- The range = the codomain
- Matrix specific theorems -> Let $T : \mathbb{R}^n \to \mathbb{R}^m$ be a linear transformation, and $A$ be standard matrix for $T$
	- $T$ maps $\mathbb{R}^n$ onto $\mathbb{R}^m$ if and only if the columns of $A$ span $\mathbb{R}^m$
		- Onto -> range = codomain
		- If our columns (vectors) span the entire output codomain, then our output (range) = codomain
	- $T$ is one-to-one 
		- One-to-one -> map every $\mathbf{x}$ to a unique $\mathbf{b}$ in the codomain
		- If and only if the equation $T(\mathbf{x}) = \mathbf{0}$ has only the trivial solution
			- This means we don't have free variables, and no extra vectors that contribute to the span of the matrix $A$
			- Therefore it's not possible to get multiple solutions, because we'd need free variables/redundant vectors to get multiple solutions
		- If and only if the columns of $A$ are linearly independent
			- Linearly independent vectors ensure a unique solution, because no extra vectors contribute to span of matrix $A$
			- Therefore it's not possible to get multiple solutions, because we'd need free variables/redundant vectors to get multiple solutions