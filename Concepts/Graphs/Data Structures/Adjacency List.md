# Adjacency List
#graph #data-structure

Representation of graph using a dictionary or an array that maps a node to the neighbors of that node. 

For the dictionary version, the keys of the dictionary represents nodes. The value of a key is an array of connected neighbors ("neighbors array").

For the array version, the indices of the array represent the nodes, and each index of the array is an array of connected neighbors ("neighbors array").

Both array and dictionary versions can also use a set to represent the "neighbors array." Using a set would create $O(1)$ lookup/creation of an edge.

### **Ex.** Directed Graph
```python
# Adjacency Dictionary
adj_dict = { 
	"A": ["B", "C"], 
	"B": ["C"], 
	"C": ["B"]
}

# Adjacency List
# Let 0 = A, 1 = B, 2 = C
adj_list = [
	[1, 2], # A = 0
	[2],    # B = 1
	[1]     # C = 2
]

print(f"adj_dict: {adj_dict}")
print(f"adj_list: {adj_list}")
```

![[Pasted image 20241217222746.png]]

### **Ex.** Undirected Graph
```python
# Adjacency Dictionary
adj_dict = { 
	"A": ["B", "C"], 
	"B": ["A"], 
	"C": ["A"]
}

# Adjacency List
# Let 0 = A, 1 = B, 2 = C
adj_list = [
	[1, 2], # A = 0
	[1],    # B = 1
	[1],    # C = 2
]

print(f"adj_dict: {adj_dict}")
print(f"adj_list: {adj_list}")
```
If the graph is undirected, then a single edge will appear twice. Notice how edge `A - C` is represented by `A: [C]` and `C: [A]` in the adjacency list.

![[Pasted image 20241217223223.png]]
## Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Space
	- $O(E + V)$
		- Unlike the [[Adjacency Matrix]] representation, we do not store every possible edge
		- Instead, for each vertex we create a new entry in our dictionary, and for each edge we add an entry into the array for a given node
			- Therefore every vertex is represented once, and every edge is represented once, leading to $O(E + V)$
			- `dict["x"] = []`
				- Adds a new vertex
			- `dict["x"].append("y")`
				- Adds a new edge from `x` to `y`
- Time
	- Lookup edge
		- $O(E)$
			- If the implementation uses an array for the "neighbors array"
		- $O(1)$
			- if the implementation uses a set for the "neighbors array"
	- Add/remove edge
		- $O(E)$
			- If the implementation uses an array for the "neighbors array"
		- $O(1)$
			- if the implementation uses a set for the "neighbors array"

# Flashcards
#flashcards/data-structures 

Adjacency list
?
- Representation of a graph using an array of the neighbors of each vertex
- Big O
	- Space $\to O(V + E)$
	- Time
		- Lookup edge $\to O(E)$
		- Add/remove edge $\to O(E)$
<!--SR:!2025-02-26,17,210-->