# Valid Sudoku
#problem #d-medium #hashing
https://leetcode.com/problems/valid-sudoku/description/

You are given a a `9 x 9` Sudoku board `board`. A Sudoku board is valid if the following rules are followed:

1. Each row must contain the digits `1-9` without duplicates.
2. Each column must contain the digits `1-9` without duplicates.
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without duplicates.

Return `true` if the Sudoku board is valid, otherwise return `false`

Note: A board does not need to be full or be solvable to be valid.

# Flashcards
#flashcards/problems 

Valid Sudoku
- You are given a a `9 x 9` Sudoku board `board`. A Sudoku board is valid if the following rules are followed:
	1. Each row must contain the digits `1-9` without duplicates.
	2. Each column must contain the digits `1-9` without duplicates.
	3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without duplicates.
- Return `true` if the Sudoku board is valid, otherwise return `false`
- Note: A board does not need to be full or be solvable to be valid.
?
- Brute force
	- Big O
		- Time $\to O(n^2)$
		- Space $\to O(n^2)$
	- Check every row, every column, and every sub-box
	- If duplicate occurs, return false
- Hashing (One-pass)
	- Big O
		- Time $\to O(n^2)$
		- Space $\to O(n^2)$
	- Maintain dictionary of sets for: `rows`, `cols`, `subboxes`
	- Iterate through each cell and insert cell into the sets:
		- `rows[r]`, `cols[c]`, `subboxes[r // 3 * 3 + c // 3]`
	- If duplicate occurs, return false