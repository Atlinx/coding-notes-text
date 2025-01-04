# Group Anagrams
#problem #d-medium #hashing
https://leetcode.com/problems/group-anagrams/

Given an array of strings `strs`, group all _anagrams_ together into sublists. You may return the output in **any order**.

An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.

# Flashcards

Group anagrams
- Given an array of strings `strs`, group all _anagrams_ together into sublists. You may return the output in **any order**.
- An **anagram** is a string that contains the exact same characters as another string, but the order of the characters can be different.
?
- Sorting
	- Big O
		- Time $\to O(n \log n)$
		- Space $\to O(n)$
	- Iterate through `strs` and increment count in dictionary using sorted string as the key
- Hashing
	- Big O
		- Time $\to O(n)$
		- Space $\to O(n)$
	- Iterate through `strs` and increment dictionary using an array representing the number of each letter