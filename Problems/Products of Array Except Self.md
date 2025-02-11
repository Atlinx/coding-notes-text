# Products of Array Except Self
#problem #d-medium #array
https://leetcode.com/problems/product-of-array-except-self/

Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

# Flashcards
#flashcards/problems 

Products of array except self
- Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.
- The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.
- You must write an algorithm that runs in `O(n)` time and without using the division operation.
?
- Prefix & suffix
	- Big O
		- Time $\to O(n)$
		- Space $\to O(n)$
	- Find product without self by multiplying product of left and right of the element
		- `prefix` array = Cumulative product from left $\to$ right
			- First element is 1, so we "offset" the array right
		- `suffix` array = Cumulative product from right $\to$ left
			- Last element is 1, so we "offset" the array left
	- `answer[i] = prefix[i] * suffix[i]`
		- `prefix[i]` = product to left of `i`
		- `suffix[i]` = product to right of `i`
	- Can further optimize by doing it all within the answer array
<!--SR:!2025-03-31,49,250-->