# Connected Graphs
#graph #concept 

- **DEFINE:** Connected graph ^connected-graph
	- A graph is connected if there exists a [[#^path|path]] between any two pairs of vertices
	- A graph with a single node is automatically connected
	- ![[Pasted image 20241217224349.png]]
		- **Ex.** The graph above is connected, because there exists a path between any two pairs of vertices on the graph. From any vertex on the graph, you can navigate to every other vertex.
	- ![[Pasted image 20241217224921.png]]
		- **Ex.** The graph above is disconnected, because the top and bottom part of the graphs are separated
- **DEFINE:** Strongly connected graph ^strongly-connected-graph
	- A graph is strongly connected if there exists a directed [[Sequences#^path|path]] between any two pairs of vertices
		- A strongly connected graph must have at least one directed cycle that runs through all of the vertices in the graph
	- This property only exists for directed graph
	- ![[Pasted image 20241218000635.png]]
		- **Ex.** The graph above is strongly connected, and the cycle is indicated by orange lines
- **DEFINE:** Complete graph ^complete-graph
	- A graph is complete if there is an edge between every possible pair of vertices
	- Properties
		- $V$ vertices
		- $E = (V - 1)^2$
	- ![[Pasted image 20241217220839.png]]
		- **Ex.** The graph above is complete
			- There exists an edge between every possible pair of vertices
- **DEFINE:** Subgraph ^subgraph
	- A new graph comprised of a subset of vertices and edges from another graph
		- Note that bringing over an edge from the original graph to a subgraph will also bring over the connected vertices to the subgraph
	- ![[Pasted image 20241217225935.png]]
		- **Ex.** In the image above, there are two graphs, $G$ and $G'$ (pronounced G prime)
			- $G'$ is a subgraph of $G$, because it takes a subset of the vertices and edges from $G$
- **DEFINE:** Induced subgraph ^induced-subgraph
	- A subgraph which includes every possible edge from the original graph given the subset of vertices used from the original graph
	- ![[Pasted image 20241217235627.png]]
		- **Ex.** In the image above, there is a graph $G$ and two subgraphs of $G$ named $H$ and $F$
			- $H$ is an induced subgraph of $G$ because it includes all possible edges between its subset of vertices
			- $F$ is not an induced subgraph of $G$ because it doesn't include all possible edges from $G$ given its subset of vertices
				- Specifically, it's missing the edge $A - B$

## Connected Components
- **DEFINE:** Connected component ^connected-component
	- A [[#^connected-graph|connected]] subgraph that is not a part of any larger connected subgraph
		- Connected components are the "islands" of of connected nodes you see in a graph
		- A connected graph itself is also one giant connected component 
	- AKA islands
	- ![[Pasted image 20241217225239.png]]
		- **Ex.** In the graph above, there are three connected components, denoted in orange, red, and yellow
- **DEFINE:** Strongly connected component ^strongly-connected-component
	- A [[#^strongly-connected-graph|strongly connected]] subgraph that is not part of any larger connected subgraph
	- ![[Pasted image 20241218002319.png]]
		- **Ex.** In the directed graph above, there are four strongly connected components, denoted in orange, red, yellow, and purple
## [[Trees]]
## [[Union Find]]
