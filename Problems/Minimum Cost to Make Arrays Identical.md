# Minimum Cost to Make Arrays Identical
https://leetcode.com/problems/minimum-cost-to-make-arrays-identical/

You are given two integer arrays `arr` and `brr` of length `n`, and an integer `k`. You can perform the following operations on `arr` _any_ number of times:

- Split `arr` into _any_ number of **contiguous** subarrays and rearrange these subarrays in _any order_. This operation has a fixed cost of `k`.
- Choose any element in `arr` and add or subtract a positive integer `x` to it. The cost of this operation is `x`.
    
Return the **minimum** total cost to make `arr` **equal** to `brr`.

A **subarray** is a contiguous **non-empty** sequence of elements within an array.

# Flashcards
#flashcards/problems 

Minimum Cost to Make Arrays Identical
- You are given two integer arrays `arr` and `brr` of length `n`, and an integer `k`. You can perform the following operations on `arr` _any_ number of times:
	- Split `arr` into _any_ number of **contiguous** subarrays and rearrange these subarrays in _any order_. This operation has a fixed cost of `k`.
	- Choose any element in `arr` and add or subtract a positive integer `x` to it. The cost of this operation is `x`.
- Return the **minimum** total cost to make `arr` **equal** to `brr`.
- A **subarray** is a contiguous **non-empty** sequence of elements within an array.
?
- Big O
	- Time $\to O(n \log n)$
	- Space $\to O(1)$
- Important conclusions
	- We effectively have two choices
		- Split array and shuffle around `arr` for cost of k + add/subtracting to reach `brr`
		- Adding/subtracting initial `arr` to `brr`
	- If we choose to shuffle `arr`, we want to create the smallest difference between each pair, therefore we should sort both `arr` and `brr`
<!--SR:!2025-03-04,22,250-->