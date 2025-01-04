# Segment Tree
#graph #data-structure 

https://cp-algorithms.com/data_structures/segment_tree.html 

This is a [[Binary Tree|binary tree]] that can quickly query for data about a range of numbers from a fixed number array. This "range" query can be the sum of the range, the min element in the range, 

Each node stores
- Range (L, R)
- Data about the the range
- Left and right child
# Big O
Let $n$ = size of array
- Space
	- $O(n)$
- Time
	- Build
		- $O(n)$
			- Create $2n$ nodes, which is $O(n)$
	- Update
		- $O(\log n)$
	- Query Range
		- $O(\log n)$
		- Search for the node within the tree that has a the specific range, and return it's value
			- Worst case we traverse the entire length of tree

# Flashcards
#flashcards/data-structures

Segment tree
?
- Binary tree that answers queries about a range of numbers within an array
	- **Ex.** Sum, min value, max value, or product of the specific range
- Big O
	- Space $\to O(n)$
		- Specifically we have $2n$ nodes in the tree
	- Time
		- Build $\to O(n)$
		- Update $\to O(\log n)$
		- Query Range $\to O(\log n)$