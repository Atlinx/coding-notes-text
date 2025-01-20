# Split Array Largest Sum
#problem #binary-search
https://leetcode.com/problems/split-array-largest-sum/description/

Given an integer array `nums` and an integer `k`, split `nums` into `k` non-empty subarrays such that the largest sum of any subarray is **minimized**.

Return _the minimized largest sum of the split_.

A **subarray** is a contiguous part of the array.

**Example 1:**
> **Input:** nums = [7,2,5,10,8], k = 2
> 
> **Output:** 18
> **Explanation:** There are four ways to split nums into two subarrays.
> The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.

**Example 2:**
> **Input:** nums = [1,2,3,4,5], k = 2
> 
> **Output:** 9
> **Explanation:** There are four ways to split nums into two subarrays.
> The best way is to split it into [1,2,3] and [4,5], where the largest sum among the two subarrays is only 9.

# Flashcards
#flashcards/problems 

Split Array Largest Sum
- Given an integer array `nums` and an integer `k`, split `nums` into `k` non-empty subarrays such that the largest sum of any subarray is **minimized**.
- Return _the minimized largest sum of the split_.
- A **subarray** is a contiguous part of the array.
?
- Binary search
	- Big O
		- Time $\to O(n \log ( \text{hi} - \text{lo}))$
		- Space $\to O(1)$
	- `lo` = min sum possible = `min(nums)`
	- `hi` = max sum possible = `max(nums)`
	- Binary search through solution space *to* find a solution that uses exactly `k` partitions
		- `mid = (lo + hi) // 2`
		- `min_partitions = minRequiredPartitions(arr, max_sum)`
			- `total = partitions = 0`
			- Iterate `arr`, `a`
				- `total += a`
				- If `total > max_sum`
					- `total = a, partitions += 1`
			- Return `partitions`
		- If `min_partitions >= k`
			- Lower partitions by raising `max_sum` $\to$ increase `lo` to `mid`
		- Else
			- Increase partitions by lowering `max_sum` $\to$ lower `hi` to `mid`
<!--SR:!2025-02-07,19,250-->