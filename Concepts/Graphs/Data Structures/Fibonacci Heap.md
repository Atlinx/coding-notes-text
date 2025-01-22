# Fibonacci Heap
#graph #data-structure 

A Fibonacci Heap is a [[Heap]] that makes push operations $O(1)$, unlike a [[Binary Heap]] which takes $O(\log n)$. This works by deferring tree operations from push to when pop is called. 

![Youtube Video](https://www.youtube.com/watch?v=6JxvKfSV9Ns)
# Flashcards
#flashcards/data-structures 

Fibonacci heap
?
- Amortizes `push` costs by incurring them during `po`
- Big O
	- Time
		- Push $\to O(1)$
		- Pop $\to O(\log n)$
		- Heapify $\to O(n)$
	- Space $\to O(n)$
<!--SR:!2025-01-30,8,250-->
