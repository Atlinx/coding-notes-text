# Minimum Cost to Cut a Stick
#problem #dynamic-programming 
https://leetcode.com/problems/minimum-cost-to-cut-a-stick/description/

Given a wooden stick of length `n` units. The stick is labelled from `0` to `n`. For example, a stick of length **6** is labelled as follows:

![[Pasted image 20250105132011.png]]

Given an integer array `cuts` where `cuts[i]` denotes a position you should perform a cut at.

You should perform the cuts in order, you can change the order of the cuts as you wish.

The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut). Please refer to the first example for a better explanation.

Return _the minimum total cost_ of the cuts.

![[Pasted image 20250105132015.png]]
# Flashcards
#flashcards/problems 

Minimum Cost to Cut a Stick
- Given a wooden stick of length `n` units. The stick is labelled from `0` to `n`. For example, a stick of length **6** is labelled as follows:
- ![[Pasted image 20250105132011.png]]
- Given an integer array `cuts` where `cuts[i]` denotes a position you should perform a cut at.
- You should perform the cuts in order, you can change the order of the cuts as you wish.
- The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut). Please refer to the first example for a better explanation.
- Return _the minimum total cost_ of the cuts.
?
- Dynamic programming
	- Big O
		- Let $m$ = number of cuts
		- Time $\to O(m^2)$
			- Iterate over solutions, and for each solution iterate over cuts
			- Cuts has small bounds < 100, therefore can be treated as fixed
		- Space $\to O(m^2)$
	- Sub-problem $\to$ minimum cost to cut stick from `a` to `b`
		- `edges = [0] + sorted(cuts) + [n]`
		- `cache` maps `[a, b] -> min_cost`
		- Note that range of `a, b` spans `edges`, rather than the entire `[0, n]`
<!--SR:!2025-01-21,9,250-->