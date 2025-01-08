# Breadth First Search
#graph #algorithm

A search algorithm that visits every node of a [[Trees|tree]]. Specifically, it visits every node at the same level first, before moving onto nodes of the next level.
## Implementation
```python
from collections import deque

# Binary Tree
tree = ["A", "B", "C", "D", "E", None, "F"]

def value(i):
	if i < 0 or i >= len(tree):
		return None
	return tree[i]

def left(i):
	return i * 2 + 1

def right(i):
	return i * 2 + 2

def parent(i):
	return i // 2



# BFS
#
# Time:  O(V + E)
# Space: O(V)
def bfs(curr):
	def visit():
		print(f"{value(curr)}")

	queue = deque([curr])
	while queue:
		curr = queue.popleft()
		if value(curr):
			visit()
			queue.append(left(curr))
			queue.append(right(curr))
			
def test_bfs():
	print(f"BFS:")
	bfs(0)
	print()


test_bfs()
```
![[Pasted image 20241218135544.png]]

# Flashcards
#flashcards/algorithms 

Breadth first search
?
- Big O
	- Time $\to O(V + E)$
	- Space $\to O(V)$
- Visit every node at the same level first, before moving onto nodes of the next level.
- Implementation
	- `queue` $\to$ stores nodes to process
	- While `queue`
		- Pop `queue`, visit node, and add children to `queue`
<!--SR:!2025-01-10,3,250-->