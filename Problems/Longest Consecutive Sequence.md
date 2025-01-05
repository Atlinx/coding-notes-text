# Longest Consecutive Sequence
#problem #hashing
https://leetcode.com/problems/longest-consecutive-sequence/description/

Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.
# Flashcards
#flashcards/problems 

Longest Consecutive Sequence
- Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._
- You must write an algorithm that runs in `O(n)` time.
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