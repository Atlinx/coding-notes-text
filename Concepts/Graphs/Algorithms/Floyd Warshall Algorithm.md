# Floyd-Warshall Algorithm
#graph #algorithm

This is an algorithm for finding the shortest path between all possible pairs of vertices. This problem is also known as "All Pairs Shortest Path" (APSP).

![Youtube Video](https://www.youtube.com/watch?v=4NQ3HnhyNfQ&t=45s)

Intuition
- Dynamic programming using
	- `dp[k][i][j]`
		- `k, i, j` represent nodes in the graph
		- `dp[k][i][j]` represents the shortest path from `i` to `j` if we include node `k`
	- We want to build shortest paths from `k = 0 to n - 1`,
- Find the shortest path between  
## Implementation

```python
class AdjMat:
	def __init__(self):
		self.adj = None    # adj matrix
		self.v_to_adj = {} # map vertex to adj index
		self.adj_to_v = [] # map adj index to vertex

	def vertices():
		return self.v_to_adj.keys()

	def set_edge(self, a, b, w):
		self.adj[self.v_to_adj[a]][self.v_to_adj[b]] = w

	def remove_edge(self, a, b, w):
		self.set_edge(a, b, float("inf"))

	def get_edge(self, a, b, w):
		return self.adj[self.v_to_adj[a]][self.v_to_adj[b]]

	def has_edge(self, a, b):
		return self.get_edge(a, b) != float("inf")

	def get_neighbors(self, a):
		for v in self.v_to_adj:
			if self.has_edge(a, v):
				yield v
	
	def __str__(self):
		SPACE = 4
		VERT_SEP, HORZ_SEP = " |", "-"
		res = "".ljust(SPACE + len(VERT_SEP))
		for v in self.adj_to_v:
			res += str(v).ljust(SPACE)
		res += "\n"
		res += "".ljust(SPACE + len(VERT_SEP))
		for v in self.adj_to_v:
			res += HORZ_SEP * SPACE
		res += "\n"
		for i, row in enumerate(self.adj):
			res += str(self.adj_to_v[i]).ljust(SPACE) + VERT_SEP
			for x in row:
				res += str(x).ljust(SPACE)
			res += "\n"
		return res

	def __repr__(self):
		return str(self)

	@staticmethod
	def from_edges(edges):
		vertices = set()
		for e_str in edges.split(", "):
			e = e_str.replace("-", "#", 1).replace(" ", "#").split("#")
			vertices.add(e[0])
			vertices.add(e[1])
		adj_to_v = sorted(vertices)
		v_to_adj = {}
		for i, v in enumerate(vertices):
			v_to_adj[v] = i
	
		adj = [[0] * len(vertices) for _ in range(len(verticies))]
		for e_str in edges.split(", "):
			e = e_str.replace("-", "#", 1).replace(" ", "#").split("#")
			adj[e[0]][e[1]] = int(e[2])
		
		adj_mat = AdjMat()
		adj_mat.adj_to_v = adj_to_v
		adj_mat.v_to_adj = v_to_adj
		adj_mat.adj = adj
		return adj_mat


A_adj = AdjMat.from_edges("A-D 4, B-A 3, B-E 2, C-B 1, C-D 6, C-F 5, D-A 7, D-E 4, E-C 5, E-F 9")
B_adj = AdjMat.from_edges("A-D 4, B-A 3, B-E 2, B-C 1, C-E 6, C-F -9, ")

print(f"A_adj:\n{A_adj}")
print(f"B_adj:\n{B_adj}")

def floyd_warshall():
	
```
![[Pasted image 20250117103016.png]]
- A $\to$ This graph has all positive edges
- B $\to$ This graph has one negative cycle
	- Nodes in this negative cycle don't have shortest paths, because they would be = $-\infty$
## Big O
- Time $\to O(V^3)$
- Space $\to O(V^2)$

# Flashcards
#flashcards/algorithms 

Floyd Warshall
?
- Find all pairs shortest paths (APSP)
- Handles negative edge weights
- Big O
	- Time $\to O(V^3)$
	- Space $\to O(V^2)$
		- DP that only depends on previous `k`, therefore no need to store all possible $V^2$
- Implementation
	- Use adjacency matrix representation, with `inf` representing no edge
	- `dp[i][j]`, `next[i][j]`
	- Setup $\to$ Iterate `i, j`, build nodes from `i -> k -> j`
		- `dp[i][j] = adj_mat[i][j]`
		- If `dp[i][j] != inf` $\to$ `next[i][j] = j`
	- Algo $\to$ Iterate `k, i, j`
		- If `dp[i][k] + dp[k][j] < dp[i][j]`:
			- `dp[i][j] = dp[i][k] + dp[k][j]`
			- `next[i][j] = next[i][k]`
	- Negative cycle $\to$ Run algo again, if improvement exists, then set `dp[i][j] = -inf`, `next[i][j] = -1`
	- Find path
		- Repeatedly traverse through `next` like a linked list
<!--SR:!2025-01-21,3,250-->