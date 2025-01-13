# Trapping Rainwater
#problem #d-hard #two-pointer 
https://neetcode.io/problems/trapping-rain-water

You are given an array non-negative integers `height` which represent an elevation map. Each value `height[i]` represents the height of a bar, which has a width of `1`.

Return the maximum area of water that can be trapped between the bars.

![[Pasted image 20250101142816.png]]
# Flashcards
#flashcards/problems

Trapping Rainwater
- You are given an array non-negative integers `height` which represent an elevation map. Each value `height[i]` represents the height of a bar, which has a width of `1`.
- Return the maximum area of water that can be trapped between the bars.
- ![[Pasted image 20250101142816.png]]
?
- Prefix suffix array
	- Big O
		- Time $\to O(n)$
		- Space $\to O(n)$
	- `prefix` array of max height from left $\to$ right
	- `suffix` array of max height from right $\to$ left
	- Iterate over all elements
		- Add `min(prefix[i] - suffix[i]) - height[i]` (Find the min height surrounding current index)
- Two pointer (left, right)
	- Big O
		- Time $\to O(n)$
		- Space $\to O(1)$
	- Track `min_height` of both pointers
	- While one pointer < other
		- Move smaller pointer
			- Update `min_height`
			- Add `height[ptr] - min_height` (Shallower heights will add water)
<!--SR:!2025-01-21,9,250-->
