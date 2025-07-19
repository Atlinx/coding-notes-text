# Probabilistic Roadmaps (PRM)[^1]'
- Planning has two phases
	- Learning phase
		- Repeatedly generate random free configurations of the robot
		- Connect configurations together using a simple motion planner
		- Builds an undirected graph
	- Query phase
		- Runs path finding algorithm from a source point to a destination point
			- **Ex.** [[Dijkstra Algorithm]]
- Query and learning phase could be interwoven (although this isn't covered in the paper)
	- **Ex.** First create a small roadmap. Then, augment/simplify the roadmap later on.

[^1]: L. E. Kavraki, P. Svestka, J. . -C. Latombe and M. H. Overmars, "Probabilistic roadmaps for path planning in high-dimensional configuration spaces," in _IEEE Transactions on Robotics and Automation_, vol. 12, no. 4, pp. 566-580, Aug. 1996, doi: [10.1109/70.508439.](https://www.cs.cmu.edu/~motionplanning/papers/sbp_papers/PRM/prmbasic_01.pdf)
