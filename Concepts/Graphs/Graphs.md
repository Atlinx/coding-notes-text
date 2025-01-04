# Graphs 🕸
#graph #concept

A graph is made up of a series of points called "vertices," and pairs of vertices called "edges." Edges connect pairs of vertices together, and is denoted by a line.

- **DEFINE:** Graph
	- A mathematical structure composed of series of points called "vertices," and pairs of vertices called "edges." Edges connect pairs of vertices together, and is denoted by a line.
	- ![[Pasted image 20241217222746.png]]
		- **Ex.** A graph with nodes `A, B, C`, and edges `A - C, A - B, B - C, C - B`

- **DEFINE:** Vertex ^vertex
	- A point in a graph
	- Often drawn as a circle in graph visualizations
		- ![[Pasted image 20241217213237.png]]
- **DEFINE:** Edge ^edge
	- A pair of points in a graph
	- An edge going from vertex `A` to vertex `B` is denoted as `A - B`
	- Represents a "connection" between two points
	- Often drawn as a line connecting two points in graph visualizations
		- ![[Pasted image 20241217213202.png]]

The "structure" of a graph is its set of vertices and edges. Two graphs are distinct if their structures are not the same.

- **DEFINE:** Distinct graph ^distinct-graph
	- A graph is distinct from another graph if the two graphs do not share the same set of vertices and edges
	- Note that moving nodes around visually does not change the "structure" of a graph.

## Degree
Vertices in a graph also has a property called a "degree", which describes how many edges are connected to itself. This can be further classified into indegree and outdegrees, which specific the number of connected edges pointing towards, and away from the vertex, respectively.

- **DEFINE:** Degree ^degree
	- Number of edges connected to a vertex
- **DEFINE:** Indegree ^indegree
	- Number of edges connected to a vertex that are pointing away from the vertex
	- Only applicable for directed graphs
- **DEFINE:** Outdegree ^outdegree
	- Number of edges connected to a vertex that are pointing to the vertex
	- Only applicable for directed graphs

![[Pasted image 20241218120835.png]]
- For example, in graph above, node `B` has
	* Degree = 5
		* Represented by the red and green edges
	* Indegree = 2
		* Represented by the red colored edges
	* Outdegree = 3
		* Represented by the green colored edges

## Directed/Undirected Graphs
Graphs can also be classified as either directed or undirected graphs. Directed graphs require their edges to have a direction, while undirected graphs have edges without directions.

- **DEFINE:** Directed graph ^directed-graph
	- A [[#Graph|graph]] whose edges have a direction
		- In a directed graph, an edge that goes from node `A` to `B` is different from an edge that goes from `B` to `A`
		- `B - A` $\neq$ `A - B`
	- Directed edges are often drawn as a line with an arrow pointing away from the start vertex, and towards the destination vertex
	- AKA di-graph, digraph
	- ![[Pasted image 20241217222746.png]]
- **DEFINE:** Undirected graph ^undirected-graph
	- A [[#Graph|graph]] whose edges don't have a direction
		- In an undirected graph, a connection from `A` to `B` and a connection from `B` to `A` are represented by a single edge
		- `B - A = A - B`
	- ![[Pasted image 20241217223223.png]]
## [[Sequences]]
## [[Connected Graphs]]

## Advanced Graph Types
- [[Eulerian Graphs]]
## Graph Representations
- [[Adjacency Matrix]]
- [[Adjacency List]]
## Algorithms
- [[Hierholzer Algorithm]]