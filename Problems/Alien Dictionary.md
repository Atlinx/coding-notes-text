# Alien Dictionary
#problem #topological-sort
https://neetcode.io/problems/foreign-dictionary

There is a foreign language which uses the latin alphabet, but the order among letters is _not_ "a", "b", "c" ... "z" as in English.

You receive a list of _non-empty_ strings `words` from the dictionary, where the words are **sorted lexicographically** based on the rules of this new language.

Derive the order of letters in this language. If the order is invalid, return an empty string. If there are multiple valid order of letters, return **any** of them.

A string `a` is lexicographically smaller than a string `b` if either of the following is true:

- The first letter where they differ is smaller in `a` than in `b`.
- There is no index `i` such that `a[i] != b[i]` _and_ `a.length < b.length`.

# Flashcards
#flashcards/problems 

Alien Dictionary
- There is a foreign language which uses the latin alphabet, but the order among letters is _not_ "a", "b", "c" ... "z" as in English.
- You receive a list of _non-empty_ strings `words` from the dictionary, where the words are **sorted lexicographically** based on the rules of this new language.
- Derive the order of letters in this language. If the order is invalid, return an empty string. If there are multiple valid order of letters, return **any** of them.
- A string `a` is lexicographically smaller than a string `b` if either of the following is true:
	- The first letter where they differ is smaller in `a` than in `b`.
	- There is no index `i` such that `a[i] != b[i]` _and_ `a.length < b.length`.
?
- Topological order
	- Big O
		- Time $\to O(n)$
			- $n$ to build adjacency list of letters, and then $O(26) = O(1)$ to search through graph of letters
		- Space $\to O(1)$
			- Even though we have adjacency list of letters, we only have fixed number of letters $O(26) = O(1)$
	- Iterate through `words`
		- Any word differs from prev/next word by one letter, and this letter presents an edge
		- Add edge to adjacency list
	- Perform topological sort on graph to find ordering
<!--SR:!2025-02-23,30,270-->

