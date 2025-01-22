# Dynamic Programming
#algorithm 

This is less of a concrete algorithm and more of a general problem solving strategy. Dynamic programming (DP) is breaking a problem down into sub-problems, solving the smaller sub-problems, and caching the results of the smaller sub-problems. This is so when you encounter the same smaller sub-problem again, you don't need to recalculate.

The dimensions of dynamic programming is the number of inputs in a sub-problem, and represents the key we cache by.
- 1-D DP $\to$ Sub-problems have 1 input
	- Cache is a dictionary or a 1-D array
- 2-D DP $\to$ Sub-problems have 2 inputs
	- Cache is a dictionary or a 2-D array
- 3-D DP $\to$ Sub-problems have 3 inputs
	- Cache is a dictionary or a 3-D array
- $\dots$

Therefore, the dimension of the dynamic programming also determines the time and space complexity, because in the worst case, we visit every sub-problem once.
- 1-D DP
	- Let $n$ = range of sub-problem input 1
	- Time/Space: $O(n)$
- 2-D DP
	- Let $n$ = range of sub-problem input 1
	- Let $m$ = range of sub-problem input 2
	- Time/Space: $O(n \times m)$
- 3-D DP
	- Let $n$ = range of sub-problem input 1
	- Let $m$ = range of sub-problem input 2
	- Let $o$ = range of sub-problem input 3
	- Time/Space: $O(n \times m \times o)$
- $\dots$

# Flashcards
#flashcards/algorithms 

Dynamic programming
?
- Breaking down problem into sub-problems, solving smaller sub-problems, and caching results of smaller sub-problems
	- Commonly used for searching all possible solutions
	- Note that given a parameter spanning `[0, n]`, if only a subset of `[0, n]` is relevant for the parameter, then create that subset and use the subset index as DP parameters
		- Instead of `dp(a, b), where a, b in [0, 1, 2, ..., 99, 100]`
		- Do `dp(a_i, b_i), where a_i, b_i in [0, 5], indexing into [0, 5, 8, 100]`
- Big O
	- Time/Space scale by dimensions
		- 1-D DP $\to O(n)$
		- 2-D DP $\to O(n \times m)$
		- 3-D DP $\to O(n \times m \times o)$
		- $\dots$
- Two Approaches
	- Top-down (Memoization) $\to$ DFS search with cache
	- Bottom-up $\to$ DP array
<!--SR:!2025-02-15,24,250-->