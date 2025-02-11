# Unique Length-3 Palindromic Subsequences
#problem #d-medium 
https://leetcode.com/problems/unique-length-3-palindromic-subsequences/description/?envType=daily-question&envId=2025-01-04

Given a string `s`, return _the number of **unique palindromes of length three** that are a **subsequence** of_ `s`.

Note that even if there are multiple ways to obtain the same subsequence, it is still only counted **once**.

A **palindrome** is a string that reads the same forwards and backwards.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

**Example 1:**
> **Input:** s = "aabca"
> **Output:** 3
> **Explanation:** The 3 palindromic subsequences of length 3 are:
>  - "aba" (subsequence of "aabca")
>  - "aaa" (subsequence of "aabca")
>  - "aca" (subsequence of "aabca")

# Flashcards
#flashcards/problems 

Unique Length-3 Palindromic Subsequences
- Given a string `s`, return _the number of **unique palindromes of length three** that are a **subsequence** of_ `s`.
- Note that even if there are multiple ways to obtain the same subsequence, it is still only counted **once**.
- A **palindrome** is a string that reads the same forwards and backwards.
- A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.
	- For example, `"ace"` is a subsequence of `"abcde"`.
?
- Big O
	- Time $\to O(26n) = O(n)$
	- Space $\to O(26) = O(1)$
- Iterate over all unique letters in `s`
	- Find first and last index of letters
	- Iterate between first and last and find unique letters for middle of palindrome
		- **NOTE:** By searching from the first and last letter, we're including as many letters as possible as candidates for the middle of the palindrome
	- `res += len(unique_letters)`
<!--SR:!2025-02-13,3,250-->