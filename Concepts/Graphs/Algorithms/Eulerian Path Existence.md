# Eulerian Path Existence
#graph #concept #algorithm 

![Youtube Video](https://www.youtube.com/watch?v=xR4sGgwtR2I&t=0s)

A [[Eulerian Graphs#^eulerian-path|Eulerian path]] exists based on the following conditions

- Undirected graphs
	- Eulerian circuit exists if...
		- Every vertex has an even [[Graphs#^degree|degree]]
		- **EXPLAIN:**
			- When traversing a circuit, every vertex in circuit must have one edge you enter to reach the vertex, and one edge to exist the vertex
				- Therefore edges in a circuit must come in pairs
			- If a vertex has an odd number of degrees, that means at some point when you enter the vertex, you have no way to move out, therefore breaking the circuit
	- Eulerian path exists if...
		- Every vertex has an even degree OR
		- Exactly two vertices have odd degrees
		- **EXPLAIN:**
			- If every vertex has an even degree, then the graph has a Eulerian circuit, which by definition is also a Eulerian path
			- If there are exactly two vertices with odd degrees (rest are even), then the odd degree vertices are the start and end points of the Eulerian path
- Directed graphs
	- Eulerian circuit exists if...
		- Every vertex has equal [[Graphs#^indegree|indegree]] and [[Graphs#^outdegree|outdegree]]
	- Eulerian path exists if...
		- At most one vertex has (outdegree - indegree) = 1 AND
			- This vertex must have one more outgoing edge than incoming edges, making this the starting point of the path
		- At most one vertex has (indegree - outdegree) = 1
			- This vertex must have one more incoming edge than outgoing edge, making this the ending point of the path

## Implementation
```python
from collections import defaultdict

# Example graphs
def init_dir(edges):
	adj = defaultdict(list)
	for e_str in edges.split(", "):
		e = e_str.split("-")
		adj[e[0]].append(e[1])
	return adj

def init_undir(edges):
	adj = init_dir(edges)
	for e_str in edges.split(", "):
		e = e_str.split("-")
		adj[e[1]].append(e[0])
	return adj
	
A_adj = init_undir("A-B, B-C, C-F, E-F, E-C, D-E, D-A")
B_adj = init_undir("A-B, B-C, C-F, E-F, D-E, A-D, D-B, D-C, B-E, E-C")
C_adj = init_undir("A-D, A-E, D-E, E-F, F-B, F-C, C-B, B-A")
D_adj = init_dir("A-D, D-E, E-C, E-F, F-C, C-B, B-A")
E_adj = init_dir("A-D, D-B, D-E, E-F, E-C, F-C, C-B, C-D, B-E, B-A")
F_adj = init_dir("A-D, A-E, D-E, E-F, F-B, F-C, C-B, B-A")



# Eulerian Path/Cycle Existence Algorithm
#
# Undirected
# Time: O(V + E)
# Space: O(1)
def undir_eulerian_path_exists(adj):
	odd_count = 0
	for node, nbs in adj.items():
		deg = 0
		for nb in nbs:
			deg += 1
		if deg % 2 == 1:
			odd_count += 1
	# Eulerian path exists if 
	#     all vertices have even degrees  OR 
	#     exactly two vertices have odd degrees
	return odd_count == 0 or odd_count == 2

# Time: O(V + E)
# Space: O(1)
def undir_eulerian_cycle_exists(adj):
	odd_count = 0
	for node, nbs in adj.items():
		deg = 0
		for nb in nbs:
			deg += 1
		if deg % 2 == 1:
			return False
	# Eulerian cycle exists if 
	#     all vertices have even degree
	return True

def test_undir(name, adj):
	print(f"{name}:")
	print(f"  Eulerian path exists? {undir_eulerian_path_exists(adj)}")
	print(f"  Eulerian cycle exists? {undir_eulerian_cycle_exists(adj)}")

# Directed
# Time: O(V + E)
# Space: O(V)
def dir_eulerian_path_exists(adj):
	indeg = defaultdict(int)
	outdeg = defaultdict(int)
	for node, nbs in adj.items():
		for nb in nbs:
			indeg[nb] += 1
			outdeg[node] += 1
	all_nodes = set(indeg.keys()) | set(outdeg.keys())
	start = None
	end = None
	for node in all_nodes:
		if indeg[node] != outdeg[node]:
			if end == None and indeg[node] - outdeg[node] == 1:
				# more inner, therefore end
				end = node
			elif start == None and indeg[node] - outdeg[node] == -1:
				# more outer, therefore start
				start = node
			else:
				# degree difference is too large
				return False
	# print(f"indeg: {indeg}\noutdeg: {outdeg}")
	# Eulerian path exists if 
	#     all vertices have equal indegree and outdegree  OR 
	#     exactly one vertex has +1 outdegree (start) 
	#         and one vertex has +1 indegree (dest)
	return (start == None and end == None) or (start != None and end != None)

# Time: O(V + E)
# Space: O(V)
def dir_eulerian_cycle_exists(adj):
	indeg = defaultdict(int)
	outdeg = defaultdict(int)
	for node, nbs in adj.items():
		for nb in nbs:
			indeg[nb] += 1
			outdeg[node] += 1
	all_nodes = set(indeg.keys()) | set(outdeg.keys())
	for node in all_nodes:
		if indeg[node] != outdeg[node]:
			# degree difference exists, therefore we can't have cycle
			return False
	# print(f"indeg: {indeg}\noutdeg: {outdeg}")
	# Eulerian cycle exists if 
	#     all vertices have equal indegree and outdegree
	return True

def test_dir(name, adj):
	print(f"{name}:")
	print(f"  Eulerian path exists? {dir_eulerian_path_exists(adj)}")
	print(f"  Eulerian cycle exists? {dir_eulerian_cycle_exists(adj)}")

# Tests
test_undir("A", A_adj)
test_undir("B", B_adj)
test_undir("C", C_adj)

test_dir("D", D_adj)
test_dir("E", E_adj)
test_dir("F", F_adj)
```
![[Pasted image 20241218213650.png]]
## Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Time
	- $O(V + E)$
		- You traverse through every edge and vertex in the tree
- Space
	- $O(V)$
		- Need to create indegree and outdegree dictionaries, which are size $V$ each
		- Afterwards, then you can traverse through the dictionaries
	- $O(1)$
		- For undirected graphs, since you can calculate degrees as you go, you don't need any additional space
# Flashcards
#flashcards/algorithms/graph

Eulerian path existence algorithm
?
- Checks if Eulerian path exists
- $O(V + E)$
- Undirected
	- Even degree $\to$ cycle
	- Exactly two odd degree $\to$ path
- Directed
	- Balanced in/out degree $\to$ cycle
	- Exactly one extra in and one extra out degree $\to$ path
<!--SR:!2025-01-05,4,270-->