# Kruskal's Algorithm
#graph #algorithm 

This is an algorithm for building a [[Trees#^minimum-spanning-tree|minimum spanning tree]] from a graph. This algorithm uses the [[Union Find|union find]] data structure.

![Youtube Video](https://www.youtube.com/watch?v=JZBQLXgSGfs)
## Implementation
```python
from collections import defaultdict
import heapq

class UnionFind:
	def __init__(self):
		self.arr = {}
		self.size = {}

	def find(self, a):
		if not a in self.arr:
			self.arr[a] = a
			self.size[a] = 1
		
		if self.arr[a] == a:
			return a
		root = self.find(self.arr[a])
		self.arr[a] = root
		return root

	def union(self, a, b) -> bool:
		a_root = self.find(a)
		b_root = self.find(b)
		if a_root == b_root:
			return False
		
		if self.size[a_root] > self.size[b_root]:
			self.arr[b_root] = a_root
		else:
			self.arr[a_root] = b_root
		return True

def init_undir(edges):
	adj = defaultdict(list)
	for e_str in edges.split(", "):
		e = e_str.replace(" ", "-").split("-")
		weight = int(e[2])
		adj[e[1]].append((e[0], weight))
		adj[e[0]].append((e[1], weight))
	return adj
	
A_adj = init_undir("A-D 3, A-E 1, D-E 5, B-E 5, B-H 2, B-C 3, B-F 3, C-H 5, F-H 4, H-G 3, G-E 2")
B_adj = init_undir("D-B 2, D-A 4, D-G 6, A-C 7, A-E 3, G-E 5, G-H 8, E-B 1, E-H 3, H-C 1, C-F 2")



# Kruskal
# 
# Time:  O(E log E)
# Space: O(E)
def kruskal(adj):
	WEIGHT, START, END = 0, 1, 2
	
	# Build edges
	edge_set = set()
	for node, neigh in adj.items():
		for dest, weight in neigh:
			# Create edge_str, with lexographically smaller node names listed
			# first, which avoids duplicates in edges dict
			node_a, node_b = node, dest
			if node > dest:
				node_a, node_b = dest, node
			edge_set.add((weight, node_a, node_b))

	# Put edges into min heap that sorts by weight
	edge_heap = list(edge_set)
	heapq.heapify(edge_heap)

	# Repeatedly pop from min heap until  
	mst = []         # MST edges
	total_weight = 0
	uf = UnionFind() # Union find of nodes
	while edge_heap:
		edge = heapq.heappop(edge_heap)
		if uf.union(edge[START], edge[END]):
			# If adding edge merges connected components,
			# then we want to use this edge 
			mst.append(edge)
			total_weight += edge[WEIGHT]
	
	return (mst, total_weight)

def test(name, adj):
	edges, weight = kruskal(adj)
	print(f"{name} MST, weight {weight}:")
	print(f"  {edges}")

test("A", A_adj)
test("B", B_adj)
```
![[Pasted image 20241219210950.png]]
## Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Space
	- $O(E)$
		- Store all edges in min heap. There are $E$ edges total.
- Time
	- $O(E \log V)$
		- $O(E + E (1 + \log E)) = O(E + E \log E) = O(E \log V^2) = O(E \cdot 2\log V) = O(E \log V)$
		- $E \log E$
			- We pop every edge from min heap, until min heap is empty
				- Popping from min heap takes $\log E$
				- We eventually pop all $E$ edges from min heap 
		- $\log E \to 2 \log V \to \log V$
			- In the worst case, we are processing a complete graph, therefore $E = V^2$
				- We can remove the exponent because of log rules then remove the coefficient because big O ignores coefficients
		- $E$
			- `heapify` and creating the `UnionFind` both take $E$ operations, therefore don't contribute to overall runtime
		- $1$
			- `uf.union` calls take around constant time (inverse ackermann) therefore it doesn't affect runtime
# Flashcards
#flashcards/algorithms/graph

Kruskal's algorithm
?
- Builds MST
- $O(E \log E = E \log V)$
	- Sort all edges, and add edges from smallest to largest, as long as edge is useful (reduces number of connected components)
	- Use UnionFind to check if edge is useful
<!--SR:!2025-02-10,23,250-->