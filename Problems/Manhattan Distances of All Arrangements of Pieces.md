# Manhattan Distances of All Arrangements of Pieces
#problem #d-hard 
https://leetcode.com/problems/manhattan-distances-of-all-arrangements-of-pieces/

You are given three integers `m`, `n`, and `k`.

There is a rectangular grid of size `m × n` containing `k` identical pieces. Return the sum of Manhattan distances between every pair of pieces over all **valid arrangements** of pieces.

A **valid arrangement** is a placement of all `k` pieces on the grid with **at most** one piece per cell.

Since the answer may be very large, return it **modulo** `109 + 7`.

The Manhattan Distance between two cells `(xi, yi)` and `(xj, yj)` is `|xi - xj| + |yi - yj|`.

**Example 1:**
> **Input:** `m = 2, n = 2, k = 2`
> **Output:** `8`
> **Explanation:**
> The valid arrangements of pieces on the board are:
> ![[Pasted image 20250120204052.png]]
> - In the first 4 arrangements, the Manhattan distance between the two pieces is 1.
> - In the last 2 arrangements, the Manhattan distance between the two pieces is 2.

Thus, the total Manhattan distance across all valid arrangements is `1 + 1 + 1 + 1 + 2 + 2 = 8`.
# Flashcards
#flashcards/problems 

Manhattan Distances of All Arrangements of Pieces
- You are given three integers `m`, `n`, and `k`.
- There is a rectangular grid of size `m × n` containing `k` identical pieces. Return the sum of Manhattan distances between every pair of pieces over all **valid arrangements** of pieces.
- A **valid arrangement** is a placement of all `k` pieces on the grid with **at most** one piece per cell.
- Since the answer may be very large, return it **modulo** `109 + 7`.
- The Manhattan Distance between two cells `(xi, yi)` and `(xj, yj)` is `|xi - xj| + |yi - yj|`.
?
- Big O
	- Time $\to O(n * m)$
		- `math.comb` itself takes $O(n)$, and since we plug in $n * m$ into `math.comb`, we get $O(n * m)$
	- Space $\to O(1)$
- For each pair, calculate Manhattan distance * number of arrangements of k each pair appears in
	- = total Manhattan distance * number of arrangements of k each pair appears in
	- $$\text{arrangements of k each pair appears in} = \binom{n * m - 2}{ k - 2}$$
		- All arrangements of $k$ that use the same pair must use 2 cells and 2 pieces
			- Number of arrangements is therefore determined by ways we can choose spots for the remaining pieces
			- We have 2 less options for our next cells, and we have k - 2 pieces left to place
	- Manhattan distance of all pairs
		- Calculating distance for every pair is too intensive
		- Instead, calculate all distance contributions from rows, and all distance contributions from columns
			- Let $n$ = number of rows, $m$ = number of columns
		- Rows:
			- Instead calculate distance that each pair of rows contributes, and then distance each column contributes
				- Given row `m`, if second row is `m`, then 0
					- If other row is `m + 1`, then 1
					- If other row is `m + 2`, then 2
					- ...
				- Arithmetic series
			- To apply this to all cells, we must consider a cell from first row and from second row
				- There are $m$ cells/columns per row
				- Therefore each row we have $m$ choices
				- Therefore, $m * m$ ways to choose a cell from first row and cell from second row
			- Final result = `arithmetic_series_sum * m * m`
			- For row in rows:
				- `tot += arithmetic_series_sum * m * m`
		- Columns:
			- Do the same as what was done for rows, except reverse variables for rows + columns
<!--SR:!2025-01-23,3,250-->