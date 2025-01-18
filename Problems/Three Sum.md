# Three Sum
#problem #d-medium #two-pointer 

Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` where `nums[i] + nums[j] + nums[k] == 0`, and the indices `i`, `j` and `k` are all distinct.

The output should _not_ contain any duplicate triplets. You may return the output and the triplets in **any order**.

**Example 1:**

```java
Input: nums = [-1,0,1,2,-1,-4]

Output: [[-1,-1,2],[-1,0,1]]
```

Copy

Explanation:  
`nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.`  
`nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.`  
`nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.`  
The distinct triplets are `[-1,0,1]` and `[-1,-1,2]`.

# Flashcards
#flashcards/problems 

Three Sum
- Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` where `nums[i] + nums[j] + nums[k] == 0`, and the indices `i`, `j` and `k` are all distinct.
- The output should _not_ contain any duplicate triplets. You may return the output and the triplets in **any order**.
?
- Two pointer (left, right)
	- Big O
		- Time $\to O(n^2)$
		- Space $\to O(1)$
	- Sort `nums`
	- Iterate over `nums`, `n`
		- `remaining_num = 0 - n`
		- Find pair that sums to remaining number using two sum
<!--SR:!2025-01-29,11,230-->