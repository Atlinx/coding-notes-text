# Random Trees
![Youtube Video](https://www.youtube.com/watch?v=Ob3BIJkQJEw)
## Random Trees
- Algorithm for exploring a state space
- Algorithm
	1. Randomly choose a destination in the state space
	2. Choose a random node in the tree to use as the source
	3. Try to move a set distance toward that destination from the source node, and create a new node
- Drawback -> Doesn't explore the state space effiicently
	- ![[Pasted image 20250717111033.png]]
## Rapidly Exploring Random Trees (RRT) [^1]
- Like the Random Tree algorithm, except it chooses the nearest node in the tree to build off of whenever we select a random destination
- Pros -> Good at exploring the state space
- Drawback -> Path that's found is not optimal, and is jagged
- ![[Pasted image 20250717111500.png]]
- We can also add an "exploration bias"
	- **Ex.** 0.5 would mean
		- Half of the time, try moving directly towards the goal
		- Half of the time, try moving randomly
## RRT* [^2]
- This is like the Rapidly exploring random tree, except it always generates the optimal path to the initial starting state whenever it adds a new node
- Pros -> Paths are closer to optimal the more points we add
- Drawback -> More expensive to compute and add new points
- ![[Pasted image 20250717112155.png]]

[^1]: LaValle SM, Kuffner JJ. Randomized Kinodynamic Planning. _The International Journal of Robotics Research_. 2001;20(5):378-400. doi:[10.1177/02783640122067453](https://doi.org/10.1177/02783640122067453)
[^2]: S. Karaman and E. Frazzoli, "Sampling-Based Algorithms for Optimal Motion Planning," The International Journal of Robotics Research, 30(7), 2011 pp. 846-894. doi:[10.1177/0278364911406761](https://arxiv.org/abs/1105.1186).