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
