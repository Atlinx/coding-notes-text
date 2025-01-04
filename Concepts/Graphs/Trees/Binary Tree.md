# Binary Tree
#graph #data-structure 

A binary tree is a tree where every node has at most two children. The two children are denoted as the left and right children based on which side of the tree they are on. Both the left and right children along with their descendants form the left and right subtrees of the current node.

![[Pasted image 20241218123755.png]]
- **Ex.** In the tree above
	- `B` is `A`'s left child
	- `C` is `A`'s right child
	- The left-subtree of `A` is comprised of `B` and its children.
	- The right-subtree of `A` is comprised of `C` and its children

<tab></tab>

- **DEFINE:** Left child ^left-child
	- The left child of a node in a binary tree
	- Forms the left subtree from itself and its descendants
- **DEFINE:** Right child ^right-child
	- The right child of a node in a binary tree
	- Forms the right subtree from itself and its descendants
- **DEFINE:** Balanced binary tree ^balanced-binary-tree
	- A binary tree is "balanced" if the height of its left and right subtrees differ by $\leq 1$ 