# Union Find
#graph #data-structure  

This is a data structure used to efficiently test for whether two elements exist within the same set. This has applications in graph theory, because its often used by algorithms — such as [[Kruskal Algorithm|Kruskal's algorithm]] — to test of whether adding an edge will cause a merging of [[Connected Graphs#^connected-component|connected components]].

This is also known as a Disjoint Set data structure.

[Princeton CS423 - UnionFind Lecture](https://www.cs.princeton.edu/courses/archive/spring13/cos423/lectures/UnionFind.pdf)

![Youtube Video](https://www.youtube.com/watch?v=0jNmHPfA_yE)
## Implementation
```python
# Implementation of Union Find
# - Array implementation
# - Path compression
# - Union by size
class UnionFind:
	def __init__(self, size):
		self.arr = [i for i in range(size)]  # Track parent of each node i
		self.size = [1 for i in range(size)] # Track tree size, initially 1

	# Time:  O(alpha(n)), approx O(1)
	# Space: O(n)
	def find(self, a):
		if self.arr[a] == a:
			return a
		# Path compression
		root = self.find(self.arr[a])
		self.arr[a] = root
		return root

	# Time: O(1)
	def union(self, a, b) -> bool:
		# Check if we can merge
		a_root = self.find(a)
		b_root = self.find(b)
		if a_root == b_root:
			return False
		
		# Merge smaller tree into larger tree
		if self.size[a_root] > self.size[b_root]:
			self.arr[b_root] = a_root
		else:
			self.arr[a_root] = b_root
		return True


uf = UnionFind(5)
# 0 and 1 should be disconnected
print(f"a: {uf.find(0)} b: {uf.find(1)}")
uf.union(0, 2)
uf.union(2, 3)
uf.union(1, 4)
# 0 and 1 should stil be disconnected
print(f"a: {uf.find(0)} b: {uf.find(1)}")
uf.union(4, 3)
# 0 and 1 should be connected now
print(f"a: {uf.find(0)} b: {uf.find(1)}")
```

The elements within Union Find can be visualized as a [[Trees#^forest|forest]], where each each has one edge.

Union Find must always have an array tracking the parents of the a given element. At the start, all elements of the array point to their own index, which effectively means every element has a [[Sequences#^self-loop|self loops]].

```
index:  0, 1, 2, 3, 4
array: [2, 3, 2, 4, 4]
```
* **Ex.** In the above example, element 0 has a parent of element 2, element 1 has a parent of element 3, etc.

```
index:  0  1  2  3  4
array: [0, 1, 2, 3, 4]
```
- **Ex.** In the above example every element is pointing to itself

>[!note]
Union Find is commonly implemented using an **array**. However you can also design the class using a **dictionary**, which makes it easier to insert arbitrary keys rather than being forced to use a number from 0 to n.
### Union
#TODO
### Find
#TODO
### Path Compression
#TODO
## Big O
Let $n$ = number of elements in Union Find.
- Space
	- $O(n)$
		- We have two arrays, one for the parents of each element, and one for the size of each element's tree. The size of both arrays is $n$. 
	- Union
		- $O(1)$
			- 
	- Find
		- $O(n)$
			- Path compression requires us to maintain a call stack in order to set every node along the path to the root node at the end of the call, as part of path compression.
- Time
	- Union
		- $O(1)$
			- We call find, which is $O(1)$, and then merge the two trees together
	- Find
		- $O(\alpha(n))$
			- $\alpha(n)$ is Inverse Ackermann function, and grows very slowly and never goes above 5, such that it is effectively a constant
			- Find is almost constant time because we implement path compression
				- Every successive call of "find" reduces the height of the tree we're searching in

# Flashcards
#flashcards/data-structures 

Union find
?
- Represents disjoint sets (separate sets)
	- Efficiently tests whether two elements exist within the same set
- Implementation
	- Use an array to store parent of a given node
		- Node represented as indices
- Big O
	- Space $\to O(n)$
	- Time
		- Union $\to O(1)$
		- Find $\to O(\log n), O(\alpha(1))$
			- $\alpha(1) \to$ Inverse Ackermann function, which can be treated as constant time
				- Result of path compression
<!--SR:!2025-01-05,3,250-->