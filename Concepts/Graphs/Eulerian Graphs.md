# Eulerian Graphs
#graph #concept

- **DEFINE:** Eulerian path ^eulerian-path
	- A [[Sequences#^trail|trail]] that traverses through every edge in a graph exactly once
		- Eulerian paths only exist for graphs that have all its edges inside a single [[Connected Graphs#^connected-component|connected component]]
			- Having separate connected components prevents the path from reaching all edges
	- AKA Eulerian trail
	![[Pasted image 20241217205327.png]]
		- **Ex.** The graph above has a Eulerian path of `B - C - B - A - C`. Notice how the path uses all possible edges in the graph exactly once.
- **DEFINE:** Eulerian circuit ^eulerian-circuit
	- A Eulerian path that is also a [[Graphs#^circuit|circuit]]
	- A Eulerian path whose starting vertex is the same as its ending vertex
	- Eulerian circuits are [[#^eulerian-path|Eulerian paths]]
- **DEFINE:** Eulerian graph ^eulerian-graph
	- A [[Graphs|graph]] that has a Eulerian path
## [[Eulerian Path Existence]]
## [[Hierholzer Algorithm|Hierholzer's Algorithm]]