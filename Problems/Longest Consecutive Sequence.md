# Longest Consecutive Sequence
#problem #hashing
https://leetcode.com/problems/longest-consecutive-sequence/description/

Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1:**
> **Input:** `nums = [100,4,200,1,3,2]`
> **Output:** `4`
> **Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4.

**Example 2:**
> **Input:** `nums = [0,3,7,2,5,8,4,6,0,1]`
> **Output:** `9`
# Flashcards
#flashcards/problems 

Longest Consecutive Sequence
- Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._
- You must write an algorithm that runs in `O(n)` time.
- **Example 1:**
	- **Input:** `nums = [100,4,200,1,3,2]`
	- **Output:** `4`
	- **Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4.
?
- Dictionary
	- Big O
		- Time $\to O(n)$
		- Space $\to O(n)$
	- Iterate through `set(nums)` and build ranges, tracking max range size
	- `range_start`, `range_end` dicts map `start -> end` and `end -> start`
- Hashing
	- Big O
		- Time $\to O(n)$
		- Space $\to O(n)$
	- `nums_set = set(nums)` $\to$ Only care about unique numbers
	- Iterate `nums_set`, `n`
		- If `n - 1` is not in set, then it's the start of a range,
			- Iterate forward until we hit end, and aggregate `max_size`
			- Checking for start avoids visiting a number more than once, therefore ensures $O(n)$ runtime
	- More memory efficient than dictionary approach
<!--SR:!2025-02-06,18,250-->