# Kth Smallest Integer in BST
#problem 
https://leetcode.com/problems/kth-smallest-element-in-a-bst/

Given the `root` of a binary search tree, and an integer `k`, return _the_ `kth` _smallest value (**1-indexed**) of all the values of the nodes in the tree_.
# Flashcards
#flashcards/problems 

Kth Smallest Integer in BST
- Given the `root` of a binary search tree, and an integer `k`, return _the_ `kth` _smallest value (**1-indexed**) of all the values of the nodes in the tree_.
?
- DFS
	- In in-order traversal, decrement `k`
	- We essentially explore the left sub-tree first, then the node itself, and then the right sub-tree
		- `k` is a `nonlocal` variable, so it's accessible from all DFS calls
<!--SR:!2025-02-19,9,230-->