# Container With Most Water
#problem #d-medium #two-pointer

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return _the maximum amount of water a container can store_.

**Notice** that you may not slant the container.

![[Pasted image 20250101113513.jpg]]

# Flashcards
#flashcards/problems 

Container with Most Water
- You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.
- Find two lines that together with the x-axis form a container, such that the container contains the most water.
- Return _the maximum amount of water a container can store_.
- ![[Pasted image 20250101113513.jpg]]
?
- Two pointer
	- Big O
		- Time $\to O(n)$
		- Space $\to O(1)$
	- Move the shorter of the left and right pointers, until they overlap
	- Aggregate `max_area` over the iteration
<!--SR:!2025-02-18,27,270-->