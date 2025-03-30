# 2 Matrix Algebra
# 2.1 Matrix Operations
- Matrix notation ^matrix-notation
	- Matrices are a grid of scalars (single numbers) of size $m \times n$
		- This means they have $m$ rows and $n$ columns
	- $$A = \begin{bmatrix}
		a_{11} &a_{12} &\dots &a_{1n} \\
		a_{21} &a_{22} &\dots &a_{2n} \\
		\vdots &\vdots &\ddots &\vdots \\
		a_{m1} & a_{m2} &\dots &a_{mn}
		\end{bmatrix} = \begin{bmatrix}\mathbf{a}_{1} &\mathbf{a}_{2} &\dots &\mathbf{a}_{n}\end{bmatrix}$$
	- The scalar entry of a matrix $A$ in the $i^{th}$ row and the $j^{th}$ column is denoted by $a_{ij}$
	- **Diagonal entries** of the matrix are $a_{11}, a_{22}, \dots$, and from the **main diagonal** of $A$
	- Matrices can also be represented a sequence of column vectors
## Sums and Scalar Multiples
- Two matrices are equal if they have the same size and if their corresponding columns are equal
- If two matrices $A, B$ are of the same size, then their sum is equal to a new matrix whose entries are the element-wise sum of the entries from $A$ and $B$
	- $$A = \begin{bmatrix}4 &0 &5 \\ -1 &3 &2\end{bmatrix} \hspace{3em} B = \begin{bmatrix}1 &1 &1 \\ 3 &5 &7\end{bmatrix} \hspace{3em} C = \begin{bmatrix}2 &-3 \\ 0 &1\end{bmatrix}$$
	- $$A + B = \begin{bmatrix}5 &1 &6 \\ 2 &8 &9\end{bmatrix}$$
	- $A + C$ is not defined, because $A + C$ have different sizes
- Scalar multiple of $rA$ is the matrix whose entries are are $r$ times the corresponding entries in $A$
	- $$A = \begin{bmatrix}2 &-3 \\ 0 &1\end{bmatrix}$$
	- $$3A = 3 \begin{bmatrix}2 &-3 \\0 &1 \end{bmatrix} = \begin{bmatrix}6 &-9 \\ 0 &3\end{bmatrix}$$
- Matrix properties -> Let $A, B, C$ be matrices of same size, and $r,s$ be scalars
	- $A + B = B + A$
	- $(A + B) + C = A + (B + C)$
	- $A + 0 = A$
	- $r(A + B) = rA + rB$
	- $(r +s)A = rA + sA$
	- $r(sA) = (rs)A$
## Matrix Multiplication
- When matrix $B$ multiplies a vector $\mathbf{x}$, it transforms $\mathbf{x}$ into vector $B\mathbf{x}$
- If this vector is in turn multiplied by a matrix $A$, the resulting vector is $A(B\mathbf{x})$
- $A(B \mathbf{x})$ is produced from $\mathbf{x}$ by a **composition** of mappings
	- ![[Pasted image 20250329160608.png]]
	- Goal -> represent composition of mappings as a single matrix $AB$, such that
		- $A(B\mathbf{x}) = (AB)\mathbf{x}$
- **DEFINE:** Matrix multiplication
	- If $A$ is an $m \times n$ matrix, and if $B$ is an $n \times p$ matrix with columns $b_{1}, \dots, b_{p}$, the nthe product $AB$ is the $m \times p$ matrix whose columns are $A\mathbf{b}_{1}, \dots, A\mathbf{b}_{p}$
		- $$AB = A\begin{bmatrix}\mathbf{b}_{1} &\mathbf{b}_{2} &\dots &\mathbf{b}_{p}\end{bmatrix} = \begin{bmatrix}A\mathbf{b}_{1} &A\mathbf{b}_{2} &\dots &A\mathbf{b}_{p}\end{bmatrix}$$
	- Each column of $AB$ is a linear combination of columns of $A$ using weights from the corresponding column of $B$
	- The resulting matrix is of size $m \times p$, having the same number of rows as $A$ and the same number of columns as $B$
	- **NOTE:** Handy shortcut for matrix multiplication 
		- Given $A$ is $n \times m$ and $B$ is $p \times q$, 
		- If the inner dimension are the same ($m = p$), then you can multiply $A$ and $B$ together
			- Otherwise, $AB$ is not defined
		- The resulting matrix will have a size $n \times q$, which is based on the outermost dimensions
		- ![[Pasted image 20250329161551.png]]
- Row-column rule for computing $AB$
	- If product $AB$ is defined, then the entry in row $i$ and column $j$ of $AB$ is the sum of the products of the corresponding entries from row $i$ in $A$, and column $j$ of $B$
		- $(AB)_{ij} = a_{i1}b_{1j} + a_{i2}b_{2j} + \dots + a_{in}b_{nj}$
		- ![[Pasted image 20250329161905.png]]
	- This is equivalent to calculating the dot product between the row $i$ from $A$ and the column $j$ from $B$
		- $(AB)_{ij} = \text{row}_{i}(A) \cdot \text{col}_{j}(B)$
## Properties of Matrix Multiplication
- Standard properties of matrix multiplication
- Let $A$ be $m \times n$ matrix, and let $B$ and $C$ have sizes for which sums and products are defined
	- $A(BC) = (AB)C$ -> associative law of multiplication
	- $A(B + C) = AB + AC$ -> left distributive law
	- $(B + C)A = BA + CA$ -> right distributive law
	- $r(AB) = (rA)B = A(rB)$ for any scalar $r$
	- $I_{m} A = A = AI_{m}$ -> identity for matrix multiplication
- Matrix multiplication warnings
	- In general $AB \neq BA$
		- If $AB = BA$, then we say $A$ and $B$ commute with one another
	- Cancellation laws do not hold for matrix multiplication
		- If $AB = AC$, it's **NOT** true that $B = C$
	- If a product $AB$ is the zero matrix, you cannot conclude in general that either $A = \mathbf{0}$ or $B = \mathbf{0}$
## Power of a Matrix
- If $A$ is an $n \times n$ matrix, $A^k$ denotes product of $k$ copies of $A$
	- $A^k = \underbrace{ A \dots A }_{ k }$
- If $A$ is nonzero, and if $\mathbf{x}$ is in $\mathbb{R}^n$, then $A^k\mathbf{x}$ is the result of left-multiplying $\mathbf{x}$ by $A$ repeatedly $k$ times
	- If $k = 0$, then $A^0\mathbf{x} = \mathbf{x}$, therefore $A^0 = I_{n}$
## Transpose of a Matrix
- **DEFINE:** Transpose ^transpose
	- Given an $m \times n$ matrix $A$, the transpose of $A$ is the $n \times m$ matrix, denoted by $A^T$, whose columns are formed from the corresponding rows of $A$
	- $$A = \begin{bmatrix}a &b \\c &d \end{bmatrix} \hspace{1em} A^T=\begin{bmatrix}a &c \\ c &d\end{bmatrix}$$
	- $$B = \begin{bmatrix}-5 &2 \\1 &-3\\0 &4\end{bmatrix} \hspace{1em} B^T=\begin{bmatrix}-5 &1 &0 \\2 &-3 &4\end{bmatrix}$$
- Transpose properties -> Let $A$ and $B$ be matrices whose sizes are appropriate for following sums and products
	- $(A^T)^T = A$
	- $(A + B)^T = A^T + B^T$
	- For any scalar $r$, $(rA)^T = rA^T$
	- $(AB)^T = B^TA^T$
		- Generalization -> The transpose of a product of matrices is equal to the product of the transpose of each matrices, in reverse order
- Usually, $(AB)^T \neq A^TB^T$ -> instead, the transpose of product of matrices equals the product of their transposes in **reverse** order
# 2.2 The Inverse of a Matrix
- What is the matrix analogue of the reciprocal (multiplicative inverse) of a nonzero number?
	- **Ex.** $a \cdot a^{-1} = 1$ and $a^{-1} \cdot a = 1$
- **DEFINE:** Invertible ^invertible
	- An $n \times n$ matrix is invertible if there is an $n \times n$ matrix $C$ such that
		- $CA = AC = I_{n}$
		- That is, when multiplied against it's original matrix, it cancels it out and yields the identity matrix $I_{n}$
	- The inverse of a matrix $A$ is denoted as $A^{-1}$, therefore
		- $A^{-1}A = AA^{-1} = I_{n}$
	- The inverse of a matrix is unique -> there exists exactly one inverse for matrices that are invertible
	- **NOTE:**
		- Invertible matrices are **square** matrices
		- Invertible matrices have linearly independent columns
		- Invertible matrices have a determinant $\neq 0$
	- The inverse of a matrix "undos" the matrix transformation operation
- **DEFINE:** Singular matrix ^singular-matrix
	- A **singular matrix** is a matrix that is not invertible
	- An invertible matrices is called a **nonsingular matrix**
- **DEFINE:** Determinant ^determinant
	- Conceptual meaning
		- In 2D space, the determinant of a matrix represents the how much the matrix scales an area in space
			- The determinant is 0 if the matrix squishes the area into a smaller dimension (into a line, or a point)
		- In 3D space, the determinant of a matrix represents how much the matrix scales a volume in space
			- The determinant is 0 if the matrix squishes the volume into a smaller dimension (into a plane, a line, or a point)
		- Whenever the transformation "flips" the space, the determine is negative
			- In 2D space -> if x and y vector
	- Given a 2D matrix $A$ where
		- $A = \begin{bmatrix}a &b \\ c &d\end{bmatrix}$
		- The determinant of $A$ is calculated as
			- $\det A = ad - bc$
	- Determinant can predict invertibility
		- If $\det A \neq 0$, then $A$ is invertible
		- $A^{-1} = \dfrac{1}{\det A}\begin{bmatrix}d &-b\\ -c &a\end{bmatrix}$
- If $A$ is invertible $n \times n$ matrix, then for each $\mathbf{b}$ in $\mathbb{R}^n$, the equation $A \mathbf{x} = \mathbf{b}$ has the unique solution $\mathbf{x} = A^{-1}\mathbf{b}$
	- That is, if you apply the inverse to the resulting vector $\mathbf{b}$ from initially applying the transformation matrix $A$, this "undos" the transformation, and we end up with $\mathbf{x}$ again
- Properties of invertible matrices
	- If $A$ is an invertible matrix, then $A^{-1}$ is invertible
		- $(A^{-1})^{-1} = A$
	- If $A$ and $B$ are $n \times n$ invertible matrices, then so is $AB$, and the inverse of $AB$ is the product product of the inverse of $A$ and $B$ in **reverse order**
		- $(AB)^{-1} = B^{-1} A^{-1}$
		- **NOTE:** This is similar to how transposes work
	- If $A$ is an invertible matrix, then so is $A^T$, and the inverse of $A^T$ is the transpose of $A^{-1}$
		- $(A^T)^{-1} = (A^{-1})^T$
		- Generalization -> The product of $n \times n$ invertible matrices is invertible, and the inverse is the product of their inverses in reverse order
## Elementary Matrix
- **DEFINE:** Elementary matrix
	- Matrix obtained by performing a single elementary row operation on an identity matrix
	- Denoted as $E$
	- ![[Pasted image 20250329171229.png]]
		- $E_{1} =$ elementary matrix that adds the first row times -4 to the third row
	- If an elementary row operation is performed on $m \times n$ matrix, the resulting matrix can be written as $EA$ where
		- The $m \times m$ matrix $E$ is created by performing the same row operation on $I_{m}$
	- Each elementary matrix $E$ is invertible, and the inverse of $E$ is the elementary matrix of the same type that transforms $E$ back into $I$
## Algorithm for Finding $A^{-1}$
- Place $A$ and $I$ side-by side to form an augmented matrix $\begin{bmatrix}A &I\end{bmatrix}$
- Then row operations on this matrix produce identical operations on $A$ and $I$
- If $A$ is row equivalent to $I$, then eventually we can transform the augmented matrix from $\begin{bmatrix}A &I\end{bmatrix} \to \begin{bmatrix}I &A^{-1}\end{bmatrix}$
# 2.3 Characterizations of Invertible Matrices
- Invertible matrix theorem
	- Let $A$ be a square $n \times n$ matrix. Then the follow statements are equivalent. That is for a given $A$, the statements are either all true or all false
	- $A$ is an invertible matrix
	- $A$ is row equivalent to the $n \times n$ identity matrix
	- $A$ has $n$ pivot positions
		- We can convert matrices to row-echelon form to quickly check if they are invertible or not
	- Equation of $A \mathbf{x} = \mathbf{0}$ has only the trivial solution
	- Columns of $A$ form a linearly independent set
	- The linear transformation $\mathbf{x} \mapsto A \mathbf{x}$ is one-to-one
	- There is an $n \times n$ matrix $C$ such that $CA = I$
	- There is an $n \times n$ matrix $D$ such that $AD = I$
	- $A^T$ is an invertible matrix
	- If $AB = I$, then $A$ and $B$ are both invertible, with $B = A^{-1}$ and $A = B^{-1}$
- Negations of the invertible matrix theorem describe the properties of singular (non-invertible) matrices
## Invertible Linear Transformations
- A linear transformation is only invertible if and only if its standard matrix $A$ is an invertible matrix
	- **Ex.** Examples of non-invertible linear transformation
		- Squishing space down into a line -> multiple points in space get squished in to the same point in the line
		- How would we undo this? We cannot map the single point on the line into multiple output points, therefore this transformation is not invertible
- ![[Pasted image 20250329172826.png]]
# 2.4 Partitioned Matrices
- Matrices are sometimes partitioned in smaller submatrices
	- ![[Pasted image 20250329173430.png]]
- Addition + scalar multiplication
	- If matrices $A$ and $B$ are the same size and partitioned the same way, then each block of $A + B$ is the matrix sum of the corresponding blocks of $A$ and $B$
	- Multiplication of a scalar is also computed block by block
- Multiplication of partitioned matrices
	- We can multiply partitioned matrices if the column partition of $A$ matches the row partition of $B$
	- ![[Pasted image 20250329173843.png]]
	- ![[Pasted image 20250329173850.png]]
	- We say partitions of $A$ and $B$ are **conformable** for **block multiplication** if we can multiply partitioned matrices
	- This is the most general way to define matrix multiplication
		- Notice how previously we use the row-column rule for computing matrix multiplication
		- This is really just a matter of partitioning the first matrix $A$ by rows, and the second matrix $B$ by columns
- Column row expansion of $AB$
	- If $A$ is $m \times n$ and $B$ is $n \times p$, then
		- $$\large \begin{align}
			AB &= \begin{bmatrix}\text{col}_{1}(A) &\text{col}_{2}(A) &\dots &\text{col}_{n}(A)\end{bmatrix} \begin{bmatrix}\text{row}_{1}(B) \\ \text{row}_{2}(B) \\ \vdots \\ \text{row}_{n}(B)\end{bmatrix} \\
			&= \text{col}_{1}(A) \text{row}_{1}(B) + \text{col}_{2}(A) \text{row}_{2}(B) + \dots + \text{col}_{n}(A) \text{row}_{n}(B)
			\end{align}$$
## Inverse of Partitioned Matrices
- **DEFINE:** Block upper triangular matrix ^block-upper-triangular-matrix
	- A partitioned matrix where the blocks form a step-like structure, similar to row-echelon form
	- $A = \begin{bmatrix}A_{11} & A_{12} \\0 &A_{22}\end{bmatrix}$
	- See textbook for example of calculating $A^{-1}$
- **DEFINE:** Block diagonal matrix ^block-diagonal-matrix
	- Partitioned matrix with zero blocks off the main diagonal (of blocks)
	- Matrix is invertible if and only if each block on the diagonal is invertible
		- $A = \begin{bmatrix}A_{11} & 0 \\0 &A_{22}\end{bmatrix}$
- When matrices are too large to fit in a computer's high-speed memory, it's partitioned
- Some high-speed computers perform matrix calculations more efficiently with partitioned matrix
# 2.5 Matrix Factorizations
- Factorization of matrix $A$ is equation that expresses $A$ as a product of two or more matrices
	- Factorization is preprocessing the data in $A$
	- By preprocessing some data, we can then run calculations in a faster way
## LU Factorization
- Solving sequence of equations with same coefficient matrix
	- **Ex.** $A \mathbf{x} = \mathbf{b}_{1} \hspace{2em} A\mathbf{x} = \mathbf{b}_{2} \hspace{2em} \dots \hspace{2em} A \mathbf{x} = \mathbf{b}_{p} \hspace{5em} (1)$
- When $A$ is invertible, one could compute $A^{-1}$, and then compute $A^{-1} \mathbf{b}_{1}$, $A^{-1}\mathbf{b}_{2}$, and so on, instead of having to run a computationally expensive row-reduction algorithm
	- However, it's more efficient to solve for the first equation in sequence $(1)$, and then obtain an LU factorization of $A$ at the same time
- **DEFINE:** LU factorization
	- Assume $A$ is $m \times n$ matrix that can be row reduced to echelon form without row interchanges
	- Then $A$ can be written in the form $A = LU$
		- ![[Pasted image 20250329180825.png]]
		- $L$ is $m \times m$ lower triangular matrix
			- Matrix $L$ is invertible, and is called a **unit lower triangular matrix**
				- All the diagonals along main diagonal is equal to $1$
				- The elements above the diagonal are $0$
				- The elements below the diagonal can be any number
		- $U$ is $m \times n$ echelon form of $A$
	- Helps us rewrite $A \mathbf{x} = \mathbf{b}$ into $L(U\mathbf{x}) = \mathbf{b}$
		- If we let $\mathbf{y} = U \mathbf{x}$, then
			- $L\mathbf{y} = \mathbf{b}$
			- $U\mathbf{x} = \mathbf{y}$
		- Therefore to solve for $\mathbf{x}$, we first solve for $\mathbf{y}$ in $L\mathbf{y} = \mathbf{b}$
		- Since $L$ is invertible, this is as easy as $\mathbf{y} = L^{-1} b$
		- Then we solve for $\mathbf{x}$ in $U\mathbf{x} = \mathbf{y}$, which is a lot simpler of a problem than $A \mathbf{x} = \mathbf{b}$
			- ![[Pasted image 20250329181116.png]]
			- We can think of the factorization as taking another route to getting to $A \mathbf{x}$
## LU Factorization Algorithm
- Algorithm for finding LU factorization
	1. Reduce $A$ to echelon form $U$ by sequence of row replacement operations, if possible
		- Not always possible, but when it is, an LU factorization exists
		- If possible, then
			- There exists some sequence of unit lower triangular elementary matrices $E_{1}, \dots, E_{p}$ such that
				- $E_{p} \dots E_{1} A = U$
			- Then, if we take the inverse of the row operations on both sides,
				- $A = (E_{p} \dots E_{1})^{-1} U = LU$
			- Therefore
				- $L = (E_{p} \dots E_{1})^{-1}$
		- ![[Pasted image 20250329182406.png]]
	2. Place entries in $L$ such that the same sequence of row operations reduces $L$ to $I$
		- $L$ will satisfy
			- $(E_{p} \dots E_{1})L = I$
		- ![[Pasted image 20250329182425.png]]
			- We simply move the pivot columns prior to applying the replacement operation into the $L$ matrix
			- Then we divide this vector by the top-most term to ensure the top of the vector is $1$
- In practice, row interchanges are almost always needed to achieve high accuracy
	- In these cases, $L$ is a **permuted lower triangular** matrix
		- This matrix is not lower triangular
		- However, a rearrangement of the rows (permuting the rows) can make the matrix unit lower triangular
# 2.8 Subspaces of $\mathbb{R}^n$
- **DEFINE:** Subspace ^subspace
	- A subspace of $\mathbb{R}^n$ is any set $H$ in $\mathbb{R}^n$ that has three properties
		- Zero vector is in $H$
		- For each $\mathbf{u}$ and $\mathbf{u}$ in $H$, the sum $\mathbf{u} + \mathbf{v}$ is in $H$
		- For each $\mathbf{u}$ in $H$ and each scalar $c$, the vector $c \mathbf{u}$ is in $H$
	- Subspace is **closed** under addition and scalar multiplication
		- Doing those operations on subspace vectors will always result in a vector that's still in the subspace
	- **Ex.** If the span of a set of vectors in $\mathbb{R}^3$ is a plane, then we can say the span of these vectors forms a subspace of $\mathbb{R}^3$
		- ![[Pasted image 20250329184513.png]]
		- All possible vectors in this subspace can be expressed as a linear combination of the span vectors
	- **Ex** If the span of a set of vectors in $\mathbb{R}^2$ is a line, then we can say the span of these vectors forms a subspace of $\mathbb{R}^2$
		- ![[Pasted image 20250329184530.png]]
- **DEFINE:** Subspace spanned/generated ^subspace-spanned
	- The subspace spanned/generated by $\mathbf{v}_{1}, \dots, \mathbf{v}_{p}$ is the subspace formed by the $\text{Span} \{ \mathbf{v}_{1}, \dots, \mathbf{v}_{p} \}$
- $\mathbb{R}^n$ is subspace of itself
- **DEFINE:** Zero subspace ^zero-subspace
	- Subspace consisting of just the zero vector itself
	- **Ex.** $\{ \mathbf{0} \}$
# Column Space and Null Space of a Matrix
- **DEFINE:** Column space ^column-space
	- Column space of an $m \times n$ matrix $A$ is the set $\text{Col } A$ of all linear combinations of the columns of $A$
		- Consider $A \mathbf{x} = \mathbf{b}$
		- All possible $\mathbf{b}$ in the equation above form the column space of $A$
	- **Ex.** If $A = \begin{bmatrix}\mathbf{a}_{1} &\dots & \mathbf{a}_{n}\end{bmatrix}$, with columns in $\mathbb{R}^m$, then
		- $\text{Col } A = \text{Span} \{ \mathbf{a}_{1}, \dots, \mathbf{a}_{n} \}$
	- $\text{Col } A = \mathbb{R}^m$ only when the $\text{Span} \{ \mathbf{a}_{1}, \dots, \mathbf{a}_{n} \} = \mathbb{R}^m$
- **DEFINE:** Null space ^null-space
	- Null space of an $m \times n$ matrix $A$ is the set $\text{Nul } A$ of all solutions of the [[1 Linear Equations#^homogenous|homogeneous equation]] $A \mathbf{x} = \mathbf{0}$
		- Consider $A \mathbf{x} = \mathbf{0}$
		- All possible $\mathbf{x}$ in the equation above form the null space of $A$
	- Null space is a subset of $\mathbb{R}^n$
## Basis for a Subspace
- **DEFINE:** Basis ^basis
	- A basis for [[#^subspace|subspace]] $H$ of $\mathbb{R}^n$ is a linearly independent set in $H$ that spans $H$
- **DEFINE:** Standard basis ^standard-basis
	- The set of column vectors from the identity matrix
	- **Ex.**
		- $\mathbf{e}_{1} = \begin{bmatrix}1 \\ 0 \\ \vdots \\0\end{bmatrix}\hspace{1em}\mathbf{e}_{2} = \begin{bmatrix}0 \\1 \\ \vdots \\ 0\end{bmatrix} \hspace{1em} \dots \hspace{1em} \mathbf{e}_{n} = \begin{bmatrix}0 \\ \vdots \\0 \\1\end{bmatrix}$
		- Standard basis for $\mathbb{R}^n = \{ \mathbf{e}_{1}, \dots, \mathbf{e}_{n} \}$
- Finding a basis for the null space of a matrix
	- Perform row-reduction to get matrix into reduced row-echelon form
	- Write the solution in [[1 Linear Equations#Parametric Vector Form|parametric vector form]].
	- The set of vectors in the parametric vector form generates $\text{Nul } A$
		- $\mathbf{0} = x_{1} \mathbf{u}_{1} + x_{2} \mathbf{u}_{2} + \dots + x_{n} \mathbf{u}_{n}$
- Finding basis for column space of matrix
	- The pivot columns of $A$ form the basis for $\text{Col } A$
		- We can find the pivot positions by reducing $A$ into an echelon form
		- Then take the columns of $A$ at those same pivot positions to form the basis for the column space of $A$
	- **NOTE:** Make sure to use the pivot columns of $A$ itself, rather than the columns of the an echelon form
		- Echelon forms have the same solutions but not necessarily the same column space
# 2.9 Dimension and Rank
## Coordinate Systems
- **DEFINE:** Coordinates
	- Suppose set $\mathcal{B} = \{ \mathbf{b}_{1}, \dots, \mathbf{b}_{p} \}$ is a basis for a subspace $H$
	- For each $\mathbf{x}$ in $H$, the **coordinates** of $\mathbf{x}$ relative to the basis $\beta$ are the weights $c_{1}, \dots, c_{p}$ such that
		- $\mathbf{x} = c_{1}\mathbf{b}_{1} + \dots + c_{p} \mathbf{b}_{p}$ and the vector in $\mathbb{R}^p$
	- The vector in $\mathbb{R}^p [\mathbf{x}]_{\beta}$ is called the **coordinate vector of $\mathbf{x}$ (relative to $\beta$)** or the **$\beta$-coordinate vector of $\mathbf{x}$**
		- $$\large [ \mathbf{x} ]_{\beta} = \begin{bmatrix}c_{1} \\ \vdots \\ c_{p}\end{bmatrix}$$
- ![[Pasted image 20250329194533.png]]
	- In the example above correspondence between $\mathbf{x} \mapsto [ \mathbf{x} ]_{\beta}$ is a one-to-one correspondence between the subspace $H$ and $\mathbb{R}^2$
	- We call this correspondence an **isomorphism**
		- $H$ is **isomorphic** to $\mathbb{R}^2$
- In general, if $\beta = \{ \mathbf{b}_{1}, \dots, \mathbf{b}_{p} \}$ is a basis for $h$, then the mapping $\mathbf{x} \to [\mathbf{x}]_{\beta}$ is a one-to-one correspondence that makes $H$ look and act the same as $\mathbb{R}^p$
	- Even though the vectors in $H$ themselves may have more than $p$ entries
## Dimension of Subspace
- **DEFINE:** Dimension ^dimension
	- Dimension of a nonzero subspace $H$ is the number of vectors in any basis for $H$
	- This is denoted by $\dim H$
	- The dimension of the zero subspace $\{ \mathbf{0} \}$ is defined to be zero
- Every basis for $\mathbb{R}^n$ consists of $n$ vectors
- A plane through the origin in $\mathbb{R}^3$ is two-dimensional
- A line through in the origin in $\mathbb{R}^3$ is one-dimensional
- **DEFINE:** Rank ^rank
	- Rank of matrix $A$ is the dimension of the column space of $A$
	- This is denoted by $\text{rank } A$
- **DEFINE:** Rank theorem ^rank-theorem
	- If a matrix $A$ has $n$ columns, then $\text{rank } A + \dim \text{Nul } A = n$
- **DEFINE:** Basis theorem ^basis-theorem
	- Let $H$ be a $p$-dimensional subspace of $\mathbb{R}^n$
	- Any linearly independent set of exactly $p$ elements in $H$ is automatically a basis for $H$
	- Any set of $p$ elements of $H$ that spans $H$ is automatically a basis for $H$
## Rank and the Invertible Matrix Theorem
- **DEFINE:** Invertible matrix theorem (continued)
	- Let $A$ be an $n \times n$ matrix. The following statements are each equivalent to the statement that $A$ is an invertible matrix.
	- The columns of $A$ form a basis of $\mathbb{R}^n$
	- $\text{Col }A = \mathbb{R}^n$
	- $\dim \text{Col } A = n$
	- $\text{rank } A = n$
	- $\text{Nul } A = \{ \mathbf{0}\}$
	- $\dim \text{Nul } A = 0$