# Two Sum
#problem #d-easy #two-pointer 
https://leetcode.com/problems/two-sum/

Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

# Flashcards
#flashcards/problems 

Two sum
- Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.
- You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.
- You can return the answer in any order.
?
- Two pointer (left, right)
	- Big O
		- Time $\to O(n)$
		- Space $\to O(1)$
	- If curr sum > target sum, move right pointer to lower the sum
	- If curr sum < target sum, move left pointer to increase sum
<!--SR:!2025-01-11,3,250-->