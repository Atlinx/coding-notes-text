# Depth First Search
#graph #algorithm #concept

A search algorithm that visits every node of a [[Trees|tree]]. Specifically, it tries to visit the left sub-tree as deep as possible, before visiting the right sub-tree.
## Ordering
For binary trees, you can visit the nodes in a specific order
- In order
	- For each node, visit the left child, itself, and the right child, in that order
- Pre order
	- For each node, visit itself, left child, and right child in that order.
- Post order
	- For each node, visit the left child, right child, and itself in that order.
## Recursive
### Implementation
```python
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



# Recursive DFS
#
# Time:  O(V + E)
# Space: O(d), where d = max depth
def dfs(curr, mode = "inorder", d = 0):
	def visit():
		print(f"{'  ' * d}{value(curr)}")
	
	if mode == "preorder":
		visit()
	if value(left(curr)):
		dfs(left(curr), mode, d + 1)
	if mode == "inorder":
		visit()
	if value(right(curr)):
		dfs(right(curr), mode, d + 1)
	if mode == "postorder":
		visit()

def test_dfs(mode = "inorder"):
	print(f"DFS: {mode}")
	dfs(0, mode)
	print()


test_dfs("inorder")
test_dfs("preorder")
test_dfs("postorder")
```
![[Pasted image 20241218135544.png]]
## Iterative
### Implementation
```python
# Binary Tree
tree = ["A", "B", "C", "D", "E", None, "F"]

def value(i):
	if i < 0 or i >= len(tree):
		return None
	return tree[i]

def left(i):
	return (i + 1) * 2 - 1

def right(i):
	return (i + 1) * 2

def parent(i):
	return i // 2



# Iterative DFS
#
# Time:  O(V + E)
# Space: O(d), where d = max depth
def dfs(root, mode = "inorder"):
	def visit():
		print(f"{value(curr)}")

	if mode == "inorder":
		stack = []
		curr = root
		while value(curr) or stack:
			if value(curr):
				# check left sub tree first
				stack.append(curr)
				curr = left(curr)
			else:
				# then visit and check right sub tree
				curr = stack.pop()
				visit()
				curr = right(curr)
	elif mode == "preorder":
		stack = []
		curr = root
		while value(curr) or stack:
			if value(curr):
				# visit, and check left sub tree 
				visit()
				stack.append(right(curr))
				curr = left(curr)
			else:
				curr = stack.pop()
	elif mode == "postorder":
		stack = [root]
		visited = [False]
		while stack:
			curr, curr_visited = stack.pop(), visited.pop()
			if value(curr):
				if curr_visited:
					visit()
				else:
					stack.append(curr)
					visited.append(True)
					stack.append(right(curr))
					visited.append(False)
					stack.append(left(curr))
					visited.append(False)

def test_dfs(mode = "inorder"):
	print(f"DFS: {mode}")
	dfs(0, mode)
	print()


test_dfs("inorder")
test_dfs("preorder")
test_dfs("postorder")
```
## Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Time
	- $O(V + E)$
		- You traverse through every edge and vertex in the tree
- Space
	- $O(V)$
		- Call stack is maximum depth of the tree, because that's the farthest you'll traverse

# Flashcards
#flashcards/algorithms/graph

Depth first search
?
- Big O
	- Time $\to O(V + E)$
	- Space $\to O(V)$
- Go as deep as possible through one child before checking other children
<!--SR:!2025-01-14,7,250-->

Depth first search: Recursive
?
- Pre order
	- `visit(), dfs(left), dfs(right)`
- In order
	- `dfs(left), visit(), dfs(right)`
- Post order
	- `dfs(left), dfs(right), visit()`
<!--SR:!2025-01-10,3,250-->

Depth first search: Iterative
?
- Pre order
	- While `curr` or `stack`
		- If `curr`, then `visit()`, add `curr.right` to `stack` and move left
		- Else, pop `stack`
- In order
	- While `curr` or `stack`
		- If `curr`, then add to stack and move left
		- Else, pop `stack`, `visit()` and move right
- Post order
	- `stack`, `visited`
	- While `stack`
		- `curr, curr_visited = stack.pop(), visited.pop()`
		- If `curr`
			- If `curr_visited`
				- `visit()`
			- Else, add `curr, curr.right, curr.left` to `stack` and `visited`, and mark `curr` as visited
<!--SR:!2025-01-10,3,250-->