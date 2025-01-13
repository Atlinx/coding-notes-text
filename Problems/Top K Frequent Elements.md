# Top K Frequent Elements
#problem #d-medium #array 
https://leetcode.com/problems/top-k-frequent-elements/description/

Given an integer array `nums` and an integer `k`, return _the_ `k` _most frequent elements_. You may return the answer in **any order**.

# Flashcards
#flashcards/problems 

Top K Frequent Elements
- Given an integer array `nums` and an integer `k`, return _the_ `k` _most frequent elements_. You may return the answer in **any order**.
- **Constraints:**
	- `1 <= nums.length <= 10^4`.
	- `-1000 <= nums[i] <= 1000`
	- `1 <= k <= number of distinct elements in nums`.
?
- Bucket Sort
	- Big O
		- Time $\to O(n)$
		- Space $\to O(n)$
	- Make `count` dictionary, and populate with count of each number
	- Make `freq` array of arrays (size of `nums`, which is max possible distinct numbers)
	- For each number `n` in count, index into `freq` and append number
		- `freq[count[n]].append(n)`
	- Iterate over `freq` from end to start to get top k
<!--SR:!2025-01-21,9,250-->