# D-ary Heap
#graph #data-structure 

A D-ary Heap is a [[Heap]] where each node has $D$ children. Implementation is very similar to [[Binary Heap]], except the cost per node is no longer constant, and scales by $D$.
## Big O
- Time
	- D-ary heap changes sink + swim costs
		- Sink $\to O(D \log_{D} n)$
		- Swim $\to O(\log_{D} n)$
	- Heapify $\to O(n)$
	- Pop $\to O(D \log_{D} n)$
		- Popping requires sinking
	- Push $\to O(\log_{D} n)$
		- Pushing requires swimming
- Space
	- $O(n)$

# Flashcards
#flashcards/data-structures 

D-ary heap
?
- A [[Heap]] where each node has $D$ children. Implementation is very similar to [[Binary Heap]], except the cost per node is no longer constant, and scales by $D$.
- Big O
	- Time
		- D-ary heap changes sink + swim costs
			- Sink $\to O(D \log_{D} n)$
			- Swim $\to O(\log_{D} n)$
		- Heapify $\to O(n)$
		- Pop $\to O(D \log_{D} n)$
			- Popping requires sinking
		- Push $\to O(\log_{D} n)$
			- Pushing requires swimming
	- Space $\to O(n)$