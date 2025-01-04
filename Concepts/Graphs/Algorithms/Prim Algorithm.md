# Prim's Algorithm
#graph #algorithm

This is an algorithm for building a [[Trees#^minimum-spanning-tree|minimum spanning tree]] from a graph.

The implementations below will use the following example graphs:
![[Pasted image 20241219210950.png]]
## Lazy Priority Queue
![Youtube Video](https://www.youtube.com/watch?v=jsmMtJpPnhU)
### Implementation
```python
from collections import defaultdict
import heapq

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


# Lazy Prim's Algorithm
# Time: O(E log E)
#
# Use an adjacency list representation.
# Use min_heap to track next shortest edge to the MST. Size of min_heap is E.
def lazy_prim(adj):
	WEIGHT, START, END = 0, 1, 2
	
	# Maintain heap of edges from a node inside the MST
	# to a node outside of the MST
	#
	# List of edges = (weight, a, b)
	edge_heap = [(None, None, next(iter(adj)))]

	mst = []
	visited = set()
	total_weight = 0
	while len(visited) < len(adj): # 							T: O(E), net = O(E 2 log E)
		# Get next shortest path to a node outside of the MST
		(weight, start, node) = heapq.heappop(edge_heap) # 		T: O(log E)
		if node in visited:
			# Don't add node if the node is already in the MST
			# (heap contained outdated node)
			continue
		
		# Add node to MST
		visited.add(node)
		if weight:
			mst.append((weight, start, node))
			total_weight += weight
		
		# Add neighbors of node to edge_heap
		for dest, weight in adj[node]:
			 heapq.heappush(edge_heap, (weight, node, dest)) # 	T: O(log E)
	
	return (mst, total_weight)


def test_lazy(name, adj):
	edges, weight = lazy_prim(adj)
	print(f"Graph {name} MST, weight {weight}:")
	print(f"  {edges}")

test_lazy("A", A_adj)
test_lazy("B", B_adj)
```
### Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Space
	- $O(E)$
		- $O(E)$
		- Store all edges in min heap. There are $E$ edges total.
- Time
	- $O(E \log V)$
		- $O(E \log E + E \log E) = O(2 E \log E) = O(E \log E) = O(E \log V)$
			- Sparse $\to O(V \log V)$
			- Dense $\to O(V^2 \log V)$
		- $E \log E$
			- We push $E$ edges to the heap
			- The min heap stores all $E$ edges, therefore it pushing takes $\log E$ per edge
		- $E\log E$
			- We pop $E$ edges from the heap
			- The min heap stores all $E$ edges, therefore it popping takes $\log E$ per edge
## Eager Priority Queue
![Youtube Video](https://www.youtube.com/watch?v=xq3ABa-px_g)
### Implementation
```python
from collections import defaultdict

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

class MaxIndexHeap:
	def __init__(self, key_value_dict = None):
		self.arr = [] # [i] = (key, value)
		self.map = {} # [key]: i
		if key_value_dict != None:
			self.arr = list(key_value_dict.items())
			for i in range(len(self.arr)):
				self.map[self.arr[i][0]] = i
		self.heapify()

	def __repr__(self):
		return f"MinHeap: {self.arr} {self.map}"
	
	def __str__(self):
		return repr(self)

	def _sink(self, i):
		if i >= len(self.arr) // 2:
			# Cannot sink leaf nodes
			return
		while True:
			ri, li = 2 * i + 1, 2 * i + 2
			if ri >= len(self.arr):
				ri = None
			if li >= len(self.arr):
				li = None
			si = None
			if (ri and li) and self._ahead_of(ri, i) and \
				self._ahead_of(li, i):
				if self._ahead_of(ri, li):
					# Right child is bigger, swap right
					si = ri
				else:
					# Left child is bigger, swap left
					si = li 
			elif li and self._ahead_of(li, i):
				# Left child is bigger, swap left
				si = li
			elif ri and self._ahead_of(ri, i):
				# Right child is bigger, swap right
				si = ri
			
			if si != None:
				# Perform swap
				
				i_key, si_key = self.arr[i][0], self.arr[si][0]
				self.arr[i], self.arr[si] = self.arr[si], self.arr[i]
				self.map[i_key], self.map[si_key] = si, i
				i = si
			else:
				# We are at the right place
				return

	def _swim(self, i):
		pi = i // 2
		while self._ahead_of(i, pi):
			# swap with parent
			i_key, pi_key = self.arr[i][0], self.arr[pi][0]
			self.arr[i], self.arr[pi] = self.arr[pi], self.arr[i]
			self.map[i_key], self.map[pi_key] = pi, i
			i = pi
			pi = i // 2

	def _ahead_of_value(self, a, b):
		return a > b

	def _ahead_of(self, a, b):
		return self._ahead_of_value(self.arr[a][1], self.arr[b][1])

	# Time: O(n)
	def heapify(self):
		for i in reversed(range(len(self.arr) // 2)):
			self._sink(i)

	# Time: O(1)
	def has(self, key) -> bool:
		return key in self.map

	# Time: O(1)
	def get(self, key):
		if key in self.map:
			return self.arr[self.map[key]][1]
		return None

	# Time: O(log n)
	#
	# Return
	# - True if created for first time
	# - False if key was updated
	def push(self, key, value):
		if key in self.map:
			self.update(key, value)
			return False
		self.arr.append((key, value))
		self.map[key] = len(self.arr) - 1
		self._swim(len(self.arr) - 1)
		return True
	
	# Time: O(log n)
	#
	# Return largest value
	def pop(self):
		if len(self.arr) == 0:
			return None
		key_value = self.arr[0]
		del self.map[key_value[0]]
		if len(self.arr) > 1:
			last_key_value = self.arr.pop()
			self.arr[0] = last_key_value
			self.map[last_key_value[0]] = 0
			self._sink(0)
		return key_value

	# Time: O(log n)
	#
	# Return
	# - True if key exists and was updated
	# - False if key does not exist
	def update(self, key, value):
		if not key in self.map:
			return False
		i = self.map[key]
		old_value = self.arr[i][1]
		self.arr[i] = (key, value)
		if self._ahead_of_value(value, old_value):
			self._swim(i)
		else:
			self._sink(i)
		return True

class MinIndexHeap(MaxIndexHeap):
	def _ahead_of_value(self, a, b):
		return a < b


# Eager Prim's Algorithm
# Time: O((V + E) log V)
#
# Use an adjacency list representation.
# Use min_heap to track next closest vertex to the MST. Size of min_heap is V.
def eager_prim(adj):
	# Maintain an indexed heap of vertices from a node inside the MST
	# to a node outside of the MST. We index by the vertex, and store edges.
	#
	# Indexed heap of edges = [vertex]: (weight, start)
	#	Interpreted as "the edge from start -> vertex has a weight"
	#													S: O(V)
	vertex_heap = MinIndexHeap({ v: (float("inf"), None) for v in adj })

	mst = []
	visited = set()
	total_weight = 0
	# MSTs must have V - 1 edges
	# 													T: net = O(V log V + E log V) = O((V + E) log V)
	while len(visited) < len(adj): # 						T: O(V), net = O(V log V)
		# Get next shortest path to a node outside of the MST
		(node, (weight, start)) = vertex_heap.pop() # 		T: O(log V)
		# Add node to MST
		visited.add(node)
		if start:
			mst.append((weight, start, node))
			total_weight += weight
		
		# Use neighbor edges to try and update vertex_heap, in case we 
		# find a new shortest edge to a vertex outside of the MST
		for dest, weight in adj[node]: #					T: O(E), net = O(E log V)
			# Don't add nodes we've already visited
			# Add to heap if
			# - The vertex doesn't exist in the heap
			# - if we have a better option
			if not dest in visited and weight < vertex_heap.get(dest)[0]:
				vertex_heap.push(dest, (weight, node)) # 		T: O(log V)
	# If len(mst) < V - 1, then the graph must be 
	# empty or disconnected, therefore no MST exists
	if len(mst) < len(adj) - 1:
		return None

	return (mst, total_weight)


def test_eager(name, adj):
	edges, weight = eager_prim(adj)
	print(f"Graph {name} MST, weight {weight}:")
	print(f"  {edges}")

test_eager("A", A_adj)
test_eager("B", B_adj)
```
### Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Space
	- $O(V)$
		- Store all vertices in the min heap. There are $V$ vertices total.
- Time
	- $O((V + E) \log V)$
		- $O(V \log V + E \log V) = O((V + E) \log V)$
			- Sparse $\to O((V + V) \log V) = O(V \log V)$
			- Dense $\to O((V + V^2) \log V) = O(V^2 \log V)$
		- $V \log V$
			- We pop $V$ vertices from the heap
			- The min heap stores all $V$ vertices, therefore it pushing takes $\log V$ per edge
		- $E \log V$
			- For each edge ($E$ total), we push/update its destination vertex on the heap
			- The min heap stores all $V$ vertices, therefore it pushing takes $\log V$ per edge
		- Note that while the eager algorithm has the same runtime as the lazy algorithm, it's better in the coefficient sense, because the actual runtime values (without big O simplification) are faster 
			- $V \log V < V \cdot 2 \log V$
## Array
Use an array to keep track of the edge from MST to min. For $V$ times, we find the next closest vertex from the array $O(V)$ and then we use the vertex's neighbors to update the array $O(V)$, leading to $O(V^2)$ runtime.
### Implementation
```python
from collections import defaultdict

def init_undir(edges_str):
	vertices = set()
	edges = []
	for e_str in edges_str.split(", "):
		e = e_str.replace(" ", "-").split("-")
		weight = int(e[2])
		edges.append((e[0], e[1], weight))
		vertices.add(e[0])
		vertices.add(e[1])
	vertices = list(vertices)
	vertices.sort()

	# Each cell in matrix represents weight of edge from vertex x to y
	# If there is no edge, then the weight is infinity
	adj_map = { v: i for i, v in enumerate(vertices)}
	adj_matrix = [[float("inf")] * len(vertices) for _ in range(len(vertices))]

	for a, b, w in edges:
		adj_matrix[adj_map[a]][adj_map[b]] = w
		adj_matrix[adj_map[b]][adj_map[a]] = w

	return (adj_map, adj_matrix)
	
A_adj = init_undir("A-D 3, A-E 1, D-E 5, B-E 5, B-H 2, B-C 3, B-F 3, C-H 5, F-H 4, H-G 3, G-E 2")
B_adj = init_undir("D-B 2, D-A 4, D-G 6, A-C 7, A-E 3, G-E 5, G-H 8, E-B 1, E-H 3, H-C 1, C-F 2")

# Adjacency matrix implementation of Prim's algorithm
#
# Time:		O(V^2)
# Space: 	O(1) if excluding adj matrix
def matrix_prim(adj):
	adj_map, adj_matrix = adj
	# Map index to vertex
	inv_adj_map = { v: k for k, v in adj_map.items() }

	# Array of all vertices in graph 
	# Each cell contains the shortest edge from MST to a given vertex
	# [vertex] = (start, weight)
	vertex_dist = [(None, float("inf")) for _ in range(len(adj_matrix))]
	visited = [False] * len(adj_matrix)
	# Mark first vertex as starting vertex for MST
	vertex_dist[0] = (None, 0)

	# Build MST vertex by vertex
	for _ in range(len(adj_matrix)): #							T: O(V), net O(V^2)
		# Get next closest vertex
		# (dest, start, weight)
		min_edge = None
		for i, (start, weight) in enumerate(vertex_dist): #		T: O(V)
			if not visited[i] and (min_edge == None or weight < min_edge[2]):
				min_edge = (i, start, weight)
		if min_edge == None:
			# We couldn't find a next smallest edge to search
			# Graph must be disconnected
			return False

		min_vertex, min_start, min_weight = min_edge
		visited[min_vertex] = True

		# Use neighboring edges to update vertex_dist array
		for i in range(len(adj_matrix)): #						T: O(V)
			edge_weight = adj_matrix[min_vertex][i]
			if not visited[i] and edge_weight < vertex_dist[i][1]:
				# This edge is better than the one we previously recorded
				#
				# We can only update a vertex's edge as long as we haven't visited
				# the vertex. After we visit the vertex it's set in stone. 
				vertex_dist[i] = (min_vertex, edge_weight) 

	edges = []
	total_weight = 0
	for i, (start, weight) in enumerate(vertex_dist): # 		T: O(V)
		if start != None:
			edges.append((inv_adj_map[i], inv_adj_map[start], weight))
			total_weight += weight

	return (edges, total_weight)


def test_matrix(name, adj):
	edges, weight = matrix_prim(adj)
	print(f"Graph {name} MST, weight {weight}:")
	print(f"  {edges}")

test_matrix("A", A_adj)
test_matrix("B", B_adj)
```
### Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Space
	- $O(1)$ but requires $O(V^2)$
		- The algorithm itself doesn't take additional space, but requires an adjacency matrix representation, which takes $O(V^2)$ space since we store every possible edges.
- Time
	- $O(V^2)$
		- We maintain `vertex_dist`, an array of shortest edges leading from MST to each vertex
		- We build the MST vertex by vertex, and there are $V$ vertices
			- First we must get the next smallest vertex to add to the tree, using the `vertex_dist` $O(V)$
			- Then we must update the `vertex_dist` with the new neighbor information $O()$
# Flashcards
#flashcards/algorithms/graph

Prim's algorithm
?
- Builds MST
- Variations
	- Array $\to O(V^2)$
	- Priority Queue $\to O(E \log V)$
		- Lazy $\to$ Insert all edges
		- Eager $\to$ Updates entries
	- Fibonacci heap $\to O(E + V \log V)$
<!--SR:!2025-01-03,1,230-->

Prim's algorithm: Priority queue
?
- $O(E \log V)$
- Lazy
	- Pop $E$ edges from `min_heap` of edges, and push $E$ edges
	- $O(E \log E + E \log E)$
- Eager $\to$ Update existing `min_heap` entry
	- Pop $V$ vertices from `min_heap` of vertices, and push/update $E$ vertices
	- $O(V \log V + E \log V)$
<!--SR:!2025-01-04,3,250-->

Prim's algorithm: Array
?
- $O(V^2)$
- Two arrays
	- `vertex_array` $\to$ Min distance to next vertex
	- `visited` $\to$ If vertex has been added to MST
- For $V$ times, get next closest vertex from `vertex_array`, mark `visited`, then update `vertex_array` using neighbors of next closest vertex
<!--SR:!2025-01-04,3,250-->