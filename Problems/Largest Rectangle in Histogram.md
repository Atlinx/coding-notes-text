# Largest Rectangle in Histogram
#problem #d-hard #stack
https://leetcode.com/problems/largest-rectangle-in-histogram/description/

You are given an array of integers `heights` where `heights[i]` represents the height of a bar. The width of each bar is `1`.

Return the area of the largest rectangle that can be formed among the bars.

![[Pasted image 20250101152054.png]]

# Flashcards
#flashcards/problems 

Largest Rectangle in Histogram
- You are given an array of integers `heights` where `heights[i]` represents the height of a bar. The width of each bar is `1`.
- Return the area of the largest rectangle that can be formed among the bars.
- ![[Pasted image 20250101152054.png]]
?
- Stack
	- Big O
		- Time $\to O(n)$
		- Space $\to O(n)$
	- Every greedy rectangle has ending edge
		- Use stack to track heights of rectangles seen so far
		- Set the start of each rectangle as early as possible
	- Iterate over `heights`, `i`
		- While head of stack's height > `heights[i]`
			- Pop stack and calculate area, as `popped_height * (i - popped_index)`
			- Set `start = popped_index`
		- Push `(start, h)` into stack
	- Parse area of remaining portion of stack
<!--SR:!2025-02-06,18,250-->