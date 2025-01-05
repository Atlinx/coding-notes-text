# N-Queens
#problem #backtracking
https://leetcode.com/problems/n-queens/

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _all distinct solutions to the **n-queens puzzle**_. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

![[Pasted image 20250105141437.png]]
# Flashcards
#flashcards

N-Queens
- The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.
- Given an integer `n`, return _all distinct solutions to the **n-queens puzzle**_. You may return the answer in **any order**.
- Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.
- ![[Pasted image 20250105141437.png]]
?
- Backtracking
	- Three sets $\to$ `cols`, `left_diag`, `right_diag`
	- DFS, `y` $\to$ One row at a time
		- If `y == n`, then we have valid solution
		- Iterate `x` from `[0, n]`
			- If none of following exist: `cols[x], left_diag[x + y], right_diag[x - y]`
				- Add queen to `x, y` and DFS `y + 1`