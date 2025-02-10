# Adjacency Matrix
#graph #data-structure 

Representation of graph using a 2D array of Booleans. Each index of an array represents a node, and each cell `matrix[x][y]` represents a connection between the `x` and `y` nodes. A cell `matrix[x][y] = True` if there is an edge from `x` to `y`. 

|     | 0   | 1   | 2   |
| --- | --- | --- | --- |
| 0   | T   | T   |     |
| 1   |     | T   |     |
| 2   |     |     |     |
### **Ex.** Directed Graph
```python
_ = False
T = True

# Adjacency Matrix
# Let 0 = A, 1 = B, 2 = C
adj_matrix = [
	[_, T, T],
	[_, _, T],
	[_, T, _]
]

print(f"adj_matrix: {adj_matrix}")
print(f"edge from A (0) to B (1)? {adj_matrix[0][1]}")
print(f"edge from B (1) to A (0)? {adj_matrix[1][0]}")
```

![[Pasted image 20241217222746.png]]

### **Ex.** Undirected Graph
```python
_ = False
T = True

# Adjacency Matrix
# Let 0 = A, 1 = B, 2 = C
adj_matrix = [
	[_, T, T],
	[T, _, _],
	[T, _, _]
]

print(f"adj_matrix: {adj_matrix}")
print(f"edge from A (0) to B (1)? {adj_matrix[0][1]}")
print(f"edge from B (1) to A (0)? {adj_matrix[1][0]}")
```
If the graph is undirected, then a single edge will appear twice to represent a connection from both sides. Notice how edge `A (0) <-> C (2)` is represented by `matrix[0][1] = True` and `matrix[1][0] = True` in the adjacency list. This property causes the adjacency matrix to be **mirrored diagonally**.

![[Pasted image 20241217223223.png]]

## Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Space
	- $O(V^2)$
		- The 2D matrix's width and height is equal to the number of vertices
			- Total area of the matrix is $w * h = V * V = V^2$
		- In other words, we store every possible edge between every two pair of vertices 
- Time
	- Lookup edge
		- $O(1)$
			- We access `matrix[x][y]` to check if there is an edge from `x` to `y`
	- Add/remove edge
		- $O(1)$
			- We set `matrix[x][y] = True/False` to add/remove an edge from `x` to `y`
# Flashcards
#flashcards/data-structures 

Adjacency matrix
?
- Representation of a graph using a 2D matrix
- Big O
	- Space $\to O(V^2)$
	- Time
		- Lookup edge $\to O(1)$
		- Add/remove edge $\to O(1)$
<!--SR:!2025-04-03,53,250-->
