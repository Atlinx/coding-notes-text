# Max Product Subarray
#problem #d-medium #sliding-window
https://leetcode.com/problems/maximum-product-subarray/description/

Given an integer array `nums`, find a **subarray** that has the largest product within the array and return it.

A **subarray** is a contiguous non-empty sequence of elements within an array.

You can assume the output will fit into a **32-bit** integer.
# Flashcards
#flashcards/problems 

Maximum Product Subarray
- Given an integer array `nums`, find a **subarray** that has the largest product within the array and return it.
- A **subarray** is a contiguous non-empty sequence of elements within an array.
- You can assume the output will fit into a **32-bit** integer.
?
- Sliding window (Specifically, Kadane's algorithm)
	- Big O
		- Time $\to O(n)$
		- Space $\to O(n)$
	- Track min and max product of current index
		- Every `i`, min and max product must end at current index, which creates three transition possibilities
			- From max product, `max_p * i`
			- From min product, `min_p * i`
			- From itself, `i`
		- `min_p = min(max_p * i, min_p * i, i)`
		- `max_p = max(max_p * i, min_p * i, i)`