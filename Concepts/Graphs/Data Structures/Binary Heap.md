# Binary Heap
#graph #data-structure 

A Binary Heap is a [[Heap]] implemented through a [[Binary Tree|binary tree]]. This binary tree is always [[Binary Tree#^balanced-binary-tree|balanced]]. If the heap is a min heap, then every node in the tree must be smaller than it's children. This means the smallest node is always the root node. For a max heap it's the opposite.  

```python
class MaxHeap:
	def __init__(self, values = None):
		self.arr = []
		if values:
			self.arr = values
		self.heapify()

	def __repr__(self):
		return f"MinHeap: {self.arr}"
	
	def __str__(self):
		return repr(self)

	def _sink(self, i):
		if i >= len(self.arr) // 2:
			# Cannot sink leaf nodes
			return
		while True:
			ri, li = 2 * i + 1, 2 * i + 2
			if ri >= len(self.arr):
				ri = None
			if li >= len(self.arr):
				li = None
			si = None
			if (ri and li) and self._ahead_of(ri, i) and \
				self._ahead_of(li, i):
				if self._ahead_of(ri, li):
					# Right child is bigger, swap right
					si = ri
				else:
					# Left child is bigger, swap left
					si = li 
			elif li and self._ahead_of(li, i):
				# Left child is bigger, swap left
				si = li
			elif ri and self._ahead_of(ri, i):
				# Right child is bigger, swap right
				si = ri
			
			if si != None:
				# Perform swap
				self.arr[i], self.arr[si] = self.arr[si], self.arr[i]
				i = si
			else:
				# We are at the right place
				return

	def _swim(self, i):
		pi = i // 2
		while self._ahead_of(i, pi):
			# swap with parent
			self.arr[i], self.arr[pi] = self.arr[pi], self.arr[i]
			i = pi
			pi = i // 2

	def _ahead_of(self, a, b):
		return self.arr[a] > self.arr[b]

	# Time: O(n)
	def heapify(self):
		for i in reversed(range(len(self.arr) // 2)):
			self._sink(i)

	# Time: O(log n)
	def push(self, value):
		self.arr.append(value)
		self._swim(len(self.arr) - 1)
	
	# Time: O(log n)
	def pop(self):
		if len(self.arr) == 0:
			return None
		value = self.arr[0]
		self.arr[0] = self.arr.pop()
		self._sink(0)
		return value

class MinHeap(MaxHeap):
	def _ahead_of(self, a, b):
		return self.arr[a] < self.arr[b]

heap = MinHeap([30, 48, 7, 3, 24, 19, 2, 3, 8, 3, 14, 16])
print(f"heap 1: {heap}")
popped = []
for i in range(5):
	popped.append(heap.pop())
print(f"popped 2: {popped}")
print(f"heap 2: {heap}")
for i in range(6):
	heap.push(i)
print(f"heap 3: {heap}")
popped.clear()
for i in range(5):
	popped.append(heap.pop())
print(f"popped 4: {popped}")
print(f"heap 4: {heap}")
```

## Big O
Let $n$ = number of elements in heap
- Space
	- $O(n)$
- Time
	- Heapify
		- $O(n)$
			- See proof below
	- Push
		- $O(\log n)$
			- Pushing adds a new element to the end of the array and then swims it up
			- Since a binary heap is balanced, swimming a node up the tree takes at most the tree height operations, therefore $\log n$ operations
	- Pop
		- $O(\log n)$
			- Popping removes the first element, replaces it with the last, and then sinks the new first element down
			- Since a binary heap is balanced, sinking a node down the tree takes at most the tree height operations, therefore $\log n$ operations

### Heapify Runtime Proof

$$\begin{align}
O(n) &= 1 \cdot \frac{n}{2^2} + 2 \cdot \frac{n}{2^3} + 3 \cdot \frac{n}{2^4} + 4 \cdot \frac{n}{2^5} + \dots \tag{1} \\
&= \sum_{k = 1}^{\infty} k \left( \frac{n}{2^{k - 1}} \right)  \tag{2} \\
&= n \sum_{k=1}^{\infty} k \left( \frac{1}{2} \right)^{k-1} \tag{3} \\
&= n \sum_{k=1}^{\infty} k x^{k-1} &x = \frac{1}{2} \tag{4}\\
&= n \frac{d}{dx} \left[ \sum_{k=0}^\infty x^k \right] \tag{5} \\
&= n \frac{d}{dx} \left[ \frac{1}{1-x} \right] \tag{6} \\
&= n \cdot \frac{1}{(1-x)^2} \tag{7} \\
&= \frac{n}{\left( 1 - \frac{1}{2} \right)^2} \tag{8} \\
&= \frac{n}{\left( \frac{1}{2} \right)^2} \\
&= \frac{n}{\frac{1}{4}} \\
&= 4n \\
&= O(n)
\end{align}
$$

**Step 1**
We start from the middle of the array and move backwards, sinking every element down. Because a heap is represented as a balanced binary tree, we can represent the array in terms of the [[Trees#^level|levels]] of the binary tree. 

For perfect binary trees, we know that the number of leaf nodes is roughly half of the total number of nodes. Specifically, $l= \dfrac{n+1}{2}$. For our purposes of calculating big O runtime, we can ignore the$+1$ constant and assume the last level contains $\dfrac{n}{2}$ nodes.

Since a each node in a binary tree can have two children, that means each complete level we add to the tree will have double the amount of nodes in the previous level. Specifically, the number of nodes in a level can be represented by $2^l$. This also means a level will always have half of the nodes in the next level. For example, $2^{l-1} = \frac{2^l}{2}$, where $l-1$ is the current level and $l$ is the next level. Therefore, levels shrink by half as we move from the lowest level to the first level.

The middle of the array represents the second to last level in the binary tree. Since we know levels shrink by half as we move up, there must be $\dfrac{n}{4}$ nodes in this level. Each node in this level can also sink at most 1 time before it hits the last level of the tree. Therefore, the total number of operations for this level is $\dfrac{n}{4} = \dfrac{n}{2^2}$.

**Step 2**
We convert this infinite series into a summation.

**Step 3**
We factor $n$ out of the series since it doesn't change within the summation.

**Step 4**
We replace the $\dfrac{1}{2}$ with a new variable $x$.

**Step 5**
We take the antiderivative of the summation.

**Step 6**
Since the summation in the previous step is a geometric series, that has an $r=\dfrac{1}{2}<1$ we can calculate the a definite sum from the infinite summation using the following formula:

$$
\sum_{k=0}^\infty ar^{k} = \frac{a}{1 - r} \hspace{2em} \text{if } r < 1
$$
**Step 7**
We simplify the derivative.

**Step 8**
We continue simplifying until we reach $4n$. Because we're calculating big O runtime, we can remove the coefficient and represent the final runtime as $O(n)$.

# Flashcards
#flashcards/data-structures 

Binary heap
?
- Retrieve smallest/largest element
- Implementation
	- Binary tree that is always balanced
	- Heap invariant $\to$ Children of a node are always smaller than
	- Commonly implemented with an array
		- Child = `2 * n`, `2 * n + 1`
		- Parent = `n // 2`
- Big O
	- Space $\to O(n)$
	- Time
		- Heapify $\to O(n)$
		- Push $\to O(\log n)$
		- Pop $\to O(\log n)$
<!--SR:!2025-01-17,10,270-->