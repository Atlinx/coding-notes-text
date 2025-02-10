# Hierholzer's Algorithm
#graph #algorithm

This is an algorithm for finding a [[Eulerian Graphs#^eulerian-path|Eulerian paths]], given a Eulerian graph.

![Finding Eulerian Paths](https://www.youtube.com/watch?v=8MpoO2zA2l4)jm
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



# Hierholzer's Algorithm
# Directed graph
# Time:  O(V + E + E + E) = O(V + E)
# Space: O(V + E)         = O(V + E)
#
# Return list of nodes representing Eularian path if there exists one
# Return None if no Eularian path exists
def hierholzer_dir(adj):
	# Verify is Eularian graph and find start node
	# Directed graph is Eularian if
	#    All balanced in-out deg (pick any start) OR
	#    exactly one vertex +1 indegree (end) and one +1 outdegree (start)
	#                                   Time:  O(V + E) (visit every V + E once)
	#                                   Space: O(V + E) (indeg outdeg store V + E)
	indeg = defaultdict(int)
	outdeg = defaultdict(int)
	has_no_edges = True
	for node, neighbors in adj.items():
		if len(neighbors) > 0:
			has_no_edges = False
		for neighbor in neighbors:
			indeg[neighbor] += 1
			outdeg[node] += 1
	if has_no_edges:
		# If we don't have any edges, then our graph is automatically Eularian
		return []

	unique_nodes = set(indeg) | set(outdeg)
	start = None
	end = None
	for node in unique_nodes:
		if indeg[node] != outdeg[node]:
			if start == None and indeg[node] - outdeg[node] == -1:
				start = node
			elif end == None and indeg[node] - outdeg[node] == 1:
				end = node
			else:
				# Difference is not -1 and 1, meaning it must be greater,
				# therefore Eulerian path/circuit does not exist
				return None
	if start == None:
		# We must have encountered a graph with balanced in-out degrees.
		# Therefore it is a cycle, and we can start anywhere
		start = unique_nodes.pop()

	# Find the Eulerian path             Time:  O(E) (visit every edge once)
	#                                    Space: O(E) (call stack memory)
	path = []
	def dfs(curr):
		# DFS into graph, and remove edges as we go (NO NEED TO TRIAL AND ERROR)
		# In postorder, add results to path
		while adj[curr]:
			next_node = adj[curr].pop()
			dfs(next_node)
		path.append(curr)
	dfs(start)
	# Reverse path, b/c we added to it in postorder
	#                                    Time: O(E) (len(path) = E)
	path.reverse()
	return path

def test_dir(name, adj):
	print(f"{name}: Found path: {hierholzer_dir(adj)}")

# hierholzer()
test_dir("D", D_adj)
test_dir("E", E_adj)
test_dir("F", F_adj)
```
![[Pasted image 20241218213650.png]]
## Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Time
	- $O(V + E)$
		- You visit every vertex and edge once via DFS.
- Space
	- $O(V + E)$
		- $V$
			- This is from the [[Eulerian Path Existence]] algorithm, which requires the creation of an indegree and outdegree dictionary to verify the existence of an Eulerian path prior to constructing it. These dictionaries have entries for all nodes, therefore have size $V$.
		- $E$
			- You must add vertices in post order, which means you must have a recursive call stack, which is of size $E$ if every edge is placed one after the other.
			- You must also create a path array to store the Eulerian path, as well as reverse it after post-order insertion is done. Eulerian paths included all edges in the graph, therefore this array is of size $E$.
# Flashcards
#flashcards/algorithms/graph

Hierholzer's algorithm
?
- Finds Eulerian path
	- Must verify graph using Eulerian path existence algorithm first
- $O(V + E)$
- DFS from start node, then backtrack and add node to path, finally reverse path array
<!--SR:!2025-03-25,44,250-->