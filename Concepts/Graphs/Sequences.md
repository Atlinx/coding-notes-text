# Sequences
#graph #concept 

A sequence is a series of vertices and adjacent edges. For [[Graphs#^directed-graph|directed graphs]], edges in a sequence must point in the same direction. Sequences can either be open or closed. 

- **DEFINE:** Closed
	- A sequence is "closed" if its starting vertex is the same as its ending vertex
- **DEFINE:** Open
	- A sequence is "open" if its starting vertex is not the same as its ending vertex

The most basic sequence is a "walks," then "trails," and finally "paths." Each type of sequence adds an additional restriction to the previous type of sequence. 

- **DEFINE:** Walk ^walk
	- A sequence of vertices in a graph where each consecutive pair of vertices are connected by an edge
	- ![[Pasted image 20241217213012.png]]
		- **Ex.** The graph above has a walk `A - C - D - G - B - E - B - E - B - H`
			- Repeated vertices are allowed (`B`)
			- Repeated edges are allowed (`B - E`)
- **DEFINE:** Trail ^trail
	- A [[#^walk|walk]] where edges are distinct (cannot repeat)
	- ![[Pasted image 20241217211207.png]]
		- **Ex.** The graph above has a trail `A - C - D - G - B - E - B - H`
			- Repeated vertices are allowed (`B`)
- **DEFINE:** Path ^path
	- A [[#^walk|walk]] where edges and vertices are distinct (cannot repeat)
	- A [[#^trail|trail]] where vertices are distinct (cannot repeat)
	- ![[Pasted image 20241217212535.png]]
		- **Ex.** The graph above has a path `A - C - D - G - B - H`
			- No repeated vertices or edges are allowed
- **DEFINE:** Self loop ^self-loop
	- A "self loop" is a edge that connects a vertex to itself
	- Can only appear in directed graphs
	- ![[Pasted image 20241217221208.png]]
		- **Ex.** Directed graph above has a self loop at vertex `A`

## Special Closed Sequences
Some closed sequences have special names.

- **DEFINE:** Circuit ^circuit
	- A closed [[#^trail|trail]]
	- A trail whose starting vertex is the same as its ending vertex
	- ![[Pasted image 20241217214135.png]]
		- **Ex.** The graph above has a circuit `C - D - G - B - E - B - C`
			- Repeated vertices are allowed (`B`)
- **DEFINE:** Cycle ^cycle
	- A closed [[#^path|path]] 
	- A cycle whose starting vertex is the same as its ending vertex
	- ![[Pasted image 20241217213927.png]]
		- **Ex.** The graph above has a cycle `B - C - D - G - B`
			- No vertices or edges are repeated
			- The cycle starts at B and ends at B
## Cyclic/Acyclic Graphs
Graphs can also be described as having or not having a cycle. 

- **DEFINE:** Cyclic graph ^cyclic-graph
	- A graph that has at least one cycle
- **DEFINE:** Acyclic graph ^acyclic-graph
	- A graph that does not have a cycle
	- All acyclic graphs are [[Trees#^forest|forests]]