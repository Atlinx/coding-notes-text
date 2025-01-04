# Topological Sort
#algorithm #graph 

Algorithm for directed, acyclic graphs that finds an ordering of nodes within the graph such that all edges point in the same direction.

Used for scheduling to perform tasks that are depended upon first.
## Implementation
```python
from collections import defaultdict

def init_dir(edges):
	adj = defaultdict(list)
	for e_str in edges.split(" "):
		e = e_str.split("-")
		adj[e[0]].append(e[1])
		adj[e[1]] # init default value for end vertex
	return adj
	
A_adj = init_dir("D-A D-B B-C B-H A-E G-E H-G H-C F-B")
B_adj = init_dir("B-D B-E A-E E-G G-H H-E C-H F-C")


# O(E + V)
def topo_sort(adj):
	res = []
	visited = set()
	visited_stack = set()
	
	def dfs(v):
		if v in visited_stack:
			return False
		if v in visited:
			return True
		visited.add(v)
		visited_stack.add(v)

		for neigh in adj[v]:
			if not dfs(neigh):
				return False

		# add elements in post-order traversal
		res.append(v)
		visited_stack.remove(v)
		return True
	
	for v in adj:
		if not v in visited:
			if not dfs(v):
				# we found cycle, topological ordering is not possible
				return []
	res.reverse()
	return res


print(f"topo_sort A: {topo_sort(A_adj)}")
print(f"topo_sort B: {topo_sort(B_adj)}")
```
![[Pasted image 20250102155841.png]]
- Graph A has no directed cycles, therefore should have a valid topological ordering
- Graph B has a directed cycle, therefore should NOT HAVE a topological ordering

## Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Space
	- $O(1)$
- Time
	- $O(V + E)$
		- We visit every vertex and edge once in our DFS search

# Flashcards
#flashcards/algorithms 

Topological Sort
?
- Big O
	- Time $\to O(V + E)$
	- Space $\to O(1)$
- Finds ordering of nodes in DAG such that all edges point in same direction
- Implementation
	- DFS graph in post-order, and add to result array
	- Return reversed result array