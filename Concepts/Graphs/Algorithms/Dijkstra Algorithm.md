# Dijkstra's Algorithm
#graph #algorithm
https://www.youtube.com/watch?v=pSqmAO-m7Lk

This is an algorithm that finds the shortest path between one node and every other node. It also only works with edge weights $\geq 0$. This algorithm is most commonly used to find the shortest path between two points.

![Youtube Video](https://www.youtube.com/watch?v=pSqmAO-m7Lk)

The implementations below will use the following example graphs, and will search from node `A`:
![[Pasted image 20250117113917.png]]
## Lazy Priority Queue
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
	
A_adj = init_undir("A-D 3, A-E 1, D-E 5, B-E 5, B-H 2, B-C 3, B-F 3, C-H 5, F-H 4, H-G 3, G-E 2, I-J 5")
B_adj = init_undir("D-B 2, D-A 4, D-G 6, A-C 8, A-E 3, G-E 5, G-H 8, E-B 1, E-H 3, H-C 1, C-F 2, I-J 8")


def lazy_dijkstra(adj, source):
	dist = {}       # shortest distance to each vertex
	prev = {}       # previous node for each node (edge from prev_node -> node)
	visited = set() # whether we've been to node before

	for a in adj:
		dist[a] = float("inf")
		prev[a] = None
	dist[source] = 0

	# min heap of next closest vertex
	min_heap = [(0, source)]
	while min_heap:
		path_dist, node = heapq.heappop(min_heap)
		visited.add(node)
		if dist[node] < path_dist:
			continue
		for neigh, neigh_weight in adj[node]:
			if neigh in visited:
				continue
			# update distance + insert into heap if there is improvement
			if path_dist + neigh_weight < dist[neigh]:
				dist[neigh] = path_dist + neigh_weight
				prev[neigh] = node
				heapq.heappush(min_heap, (path_dist + neigh_weight, neigh))
	return (dist, prev)


def find_path(prev, source, dest):
	# work backwards, traversing through prev_arr
	curr = dest
	path = [dest]
	while curr != source:
		if not curr in prev:
			return []
		curr = prev[curr]
		path.append(curr)
	path.reverse()
	return path

def test_lazy(name, adj, source):
	dist, prev = lazy_dijkstra(adj, source)
	print(f"Graph {name}, finding paths from {source}")
	print(f"  dist: {dist}")
	
	def test_path(dest):
		path = find_path(prev, source, dest)
		print(f"  {source} -> {dest}: {path}")

	test_path("B")
	test_path("C")
	test_path("D")
	test_path("E")
	test_path("F")
	test_path("G")
	test_path("H")
	test_path("I")
	test_path("J")

test_lazy("A", A_adj, "A")
test_lazy("B", B_adj, "A")
```
### Big O
- Let $V$ = number of vertices in graph, $E$ = number of edges in graph
	- Time
		- $O(E \log V)$
			- $O(E \log E + E \log E) = O(2 E \log E) = O(E \log V^2) = O(E \log V)$
				- Heap stores edges ($E$)
				- Visit every edge once ($E$)
					- Every edge gets pushed once to heap ($E \log E$)
					- Every vertex gets popped once from heap ($E \log E$)
	- Space
		- $O(V + E)$
			- Heap stores edges ($E$)
			- `dist` and `prev` store vertices ($V$)
## Eager Priority Queue
Eager version is like the lazy version except we don't blindly add neighboring edges to the heap. Instead, we only want to have one instance of a node in the heap, and update the instance if we find a path to the node.
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
	
A_adj = init_undir("A-D 3, A-E 1, D-E 5, B-E 5, B-H 2, B-C 3, B-F 3, C-H 5, F-H 4, H-G 3, G-E 2, I-J 5")
B_adj = init_undir("D-B 2, D-A 4, D-G 6, A-C 8, A-E 3, G-E 5, G-H 8, E-B 1, E-H 3, H-C 1, C-F 2, I-J 8")

class MaxIndexHeap:
	def __init__(self, key_value_dict = None):
		self.arr = [] # [i] = (key, value)
		self.map = {} # [key]: i
		if key_value_dict != None:
			self.arr = list(key_value_dict.items())
			for i in range(len(self.arr)):
				self.map[self.arr[i][0]] = i
		self.heapify()

	def __len__(self):
		return len(self.arr)

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
		else:
			self.arr.pop()
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


def eager_dijkstra(adj, source):
	dist = {}       # shortest distance to each vertex
	prev = {}       # previous node for each node (edge from prev_node -> node)
	visited = set() # whether we've been to node before

	for a in adj:
		dist[a] = float("inf")
		prev[a] = None
	dist[source] = 0

	# min heap of next closest vertex
	# vertex -> path to vertex from source
	min_heap = MinIndexHeap({ source: 0 })
	while len(min_heap) > 0:
		node, path_dist = min_heap.pop()
		visited.add(node)
		if dist[node] < path_dist:
			continue
		for neigh, neigh_weight in adj[node]:
			if neigh in visited:
				continue
			# update distance + insert into heap if there is improvement
			if path_dist + neigh_weight < dist[neigh]:
				dist[neigh] = path_dist + neigh_weight
				prev[neigh] = node
				# push/update neighbor
				min_heap.push(neigh, path_dist + neigh_weight)
	return (dist, prev)


def find_path(prev, source, dest):
	# work backwards, traversing through prev_arr
	curr = dest
	path = [dest]
	while curr != source:
		if not curr in prev:
			return []
		curr = prev[curr]
		path.append(curr)
	path.reverse()
	return path

def test_eager(name, adj, source):
	dist, prev = eager_dijkstra(adj, source)
	print(f"Graph {name}, finding paths from {source}")
	print(f"  dist: {dist}")
	
	def test_path(dest):
		path = find_path(prev, source, dest)
		print(f"  {source} -> {dest}: {path}")

	test_path("B")
	test_path("C")
	test_path("D")
	test_path("E")
	test_path("F")
	test_path("G")
	test_path("H")
	test_path("I")
	test_path("J")

test_eager("A", A_adj, "A")
test_eager("B", B_adj, "A")
```
### Big O
- Let $V$ = number of vertices in graph, $E$ = number of edges in graph
	- Time
		- $O(E \log V)$
			- $O(E \log V + V \log V) = O(E \log V)$
				- Heap stores vertices ($V$)
				- Visit every edge and potentially push vertex to heap ($E \log V$)
				- Every vertex gets popped once from heap ($V \log V$)
				- Worst case $E = V^2$, therefore $E\log V > V\log V$
	- Space
		- $O(V)$
			- Heap stores vertices ($V$)
			- `dist` and `prev` store vertices ($V$)
# Flashcards
#flashcards/algorithms 

Dijkstra's algorithm
?
- Algorithm for finding shortest path, only works on non-negative degree graphs
- Base implementation
	- `min_heap` $\to$A heap of next shortest path to explore, `min_heap = [(source, 0)]`
	- `dist` $\to$ array of shortest path distances to each vertex, `dist[source] = 0`
	- `prev` $\to$ array of previous vertex for each vertex (stores shortest path search tree)
	- `visited` $\to$ visited set
	- While `min_heap`
		- `path_dist, node = min_heap.pop()`
		- `visited.add(node)`
		- For neighbor
			- If `path_dist + neigh.weight < dist[neigh]`
				- `dist[neigh] = path_dist + neigh.weight`
				- `prev[neigh] = node`
				- `min_heap.push(neigh)`
	- Find path $\to$ Start at destination, and then visit nodes backwards using `prev`
- Variations
	- Binary heap $\to O(E \log V)$
		- Lazy
		- Eager
	- D-ary $\to O(E \log_{E / V} V)$
		- Dijkstra's algorithm tends to have more decrease key operations than pops
		- Number of children per node is set to $\dfrac{E}{V}$ to make decrease key more efficient
	- Fibonacci $\to O(E + V \log V)$
		- Tends to have a large constant overhead in practice
<!--SR:!2025-01-21,3,250-->

Dijkstra's algorithm: Priority queue
?
- Lazy $\to O(E \log E + E \log E)$
	- `min_heap` stores edges
- Eager $\to O(E \log V + V \log V)$
	- `min_heap` stores only vertices
<!--SR:!2025-01-21,3,250-->