# Minimum Interval to Include Each Query
#problem #d-hard #interval #line-sweep
https://leetcode.com/problems/minimum-interval-to-include-each-query/description/

You are given a 2D integer array `intervals`, where `intervals[i] = [left_i, right_i]` represents the `ith` interval starting at `left_i` and ending at `right_i` **(inclusive)**.

You are also given an integer array of query points `queries`. The result of `query[j]` is the **length of the shortest interval** `i` such that `left_i <= queries[j] <= right_i`. If no such interval exists, the result of this query is `-1`.

Return an array `output` where `output[j]` is the result of `query[j]`.

Note: The length of an interval is calculated as `right_i - left_i + 1`.

# Flashcards
#flashcards/problems 

Minimum Interval to Include Each Query
- You are given a 2D integer array `intervals`, where `intervals[i] = [left_i, right_i]` represents the `ith` interval starting at `left_i` and ending at `right_i` **(inclusive)**.
- You are also given an integer array of query points `queries`. The result of `query[j]` is the **length of the shortest interval** `i` such that `left_i <= queries[j] <= right_i`. If no such interval exists, the result of this query is `-1`.
- Return an array `output` where `output[j]` is the result of `query[j]`.
- Note: The length of an interval is calculated as `right_i - left_i + 1`.
?
- Line sweep
	- Big O
		- Time $\to O(q \log n + n \log n)$
		- Space $\to O(n)$
		- where
			- $q$ = number of queries
			- $n$ = number of intervals
	- Sort intervals by starting position
	- `i` = next interval to parse
	- `min_end_heap` = heap of ending intervals, sorted by smallest $\to$ largest size
	- Iterate sorted queries by position, `q`
		- While `q` is ahead of `intervals[i].start`
			- Add next interval `(size, end)` to `min_end_heap`
		- While `min_end_heap[0].end` is before `q`
			- Pop heap
		- Top of `min_end_heap` is answer to `q`
<!--SR:!2025-01-19,5,230-->