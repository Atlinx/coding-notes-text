# Trees
#graph #concept 

- **DEFINE:** Tree ^tree
	- A graph where there's a single path connecting any two pairs of vertices
		- Trees are [[Sequences#^acyclic-graph|acyclic graphs]]
	- ![[Pasted image 20241218003122.png]]
		- **Ex. A.** The graph above is a tree
	- Trees are typically drawn by pinning one of the vertices as the "root" vertex and draws connected vertices branching out from below
	- ![[Pasted image 20241218003426.png]]
		- **Ex. B.** The graph above is the same as one shown in **Ex. A**, except it pins the `C` as the root vertex and draws the remaining connected vertices below
- **DEFINE:** Spanning tree ^spanning-tree
	- A subgraph that contains all the vertices of the original graph and enough edges to form tree
		- There's a single path connecting any two pairs of vertices
		- There can be multiple spanning trees per subgraph
	- ![[Pasted image 20241218003018.png]]
		- **Ex.** In the graph above, one possible spanning tree is denoted in orange
- **DEFINE:** Minimum spanning tree ^minimum-spanning-tree
	- A spanning tree of a graph whose edge weights have the smallest possible sum
- **DEFINE:** Forest ^forest
	- A forest is a graph that contains multiple separate trees
		- Each tree forms its own [[Connected Graphs#^connected-component|connected component]]
		- Forests are [[Sequences#^acyclic-graph|acyclic graphs]]
	- ![[Pasted image 20241218122655.png]]
		- **Ex.** The graph above is a forest, made up of three separate trees denoted by the colors red, orange, and yellow
	
## Properties
Vertices in trees are also known as nodes. As mentioned before, trees tend to have a single node — called the "root" node — which is the start of the tree.

![[Pasted image 20241218125009.png]]
- **DEFINE:** Subtree ^subtree
	- A [[Connected Graphs#^subgraph|subgraph]] of a tree, which also forms a tree
- **DEFINE:** Internal node ^internal-node
	- A node in a tree that has at least one child
- **DEFINE:** Leaf node ^leaf-node
	- A node in a tree that does not have any children
	- These nodes serve as the end of a tree branch, similar to how leaves in real life sit at the ends of branches.

![[Pasted image 20241218130933.png]]
In the example above, we have a tree with a branching factor of 2, because its nodes have at most 2 children. The branches (edges) are denoted in purple, and the various levels of the tree are denoted by the colored rectangles. Notice how nodes of the same level have the same depth, which is the number of edges from the root node.

- **DEFINE:** Level ^level
	- Nodes in a tree that are the same number of edges away from the root node
		- **Ex.** 
			- Level 1 = All nodes 1 edge away from the root node
			- Level 2 = All nodes 2 edges away from the root node
			- Level 3 = All nodes 3 edges away from the root node
	- This is the same as the depth of a node
- **DEFINE:** Depth ^depth
	- Number of edges between a node from the root node
	- This is the same as the level of a node
- **DEFINE:** Branching factor ^branching-factor
	- The maximum number of children any node can have in a given tree
- **DEFINE:** Branch ^branch
	- The edges within a tree
- **DEFINE:** Height ^height
	- The length of the longest path within the tree
	- The largest depth or level within the tree

![[Pasted image 20241218130232.png]]
Terminology for trees borrow heavily from the idea of "family trees," therefore a lot of family vocabulary is used to describe various node relationships.

- **DEFINE:** Parent ^parent
	- The connected  node that is one edge closer to the root node 
	- Every node in tree has one parent, except for the root node
- **DEFINE:** Child ^child
	- A connected node that is one edge farther from the root node
	- A node can have zero or more children
- **DEFINE:** Sibling ^sibling
	- Nodes that have the same level are considered siblings of each other
- **DEFINE:** Ancestor ^ancestor
	- A connected node that is closer to the root node
	- **Ex.** Parents, grandparents, great-grandparents 
- **DEFINE:** Descendant ^descendant
	- A connected node that is farther from the root node
	- **Ex.** Children, grandchildren, great-grandchildren
## Types
- [[Binary Tree]]
- N-ary tree
- 2 - 3 tree
- Red black trees
## [[Kruskal Algorithm]]
