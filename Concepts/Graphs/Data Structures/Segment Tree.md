# Segment Tree
#graph #data-structure 

https://cp-algorithms.com/data_structures/segment_tree.html 

This is a [[Binary Tree|binary tree]] that can quickly query for data about a range of numbers from a fixed number array. This "range" query can be the sum of the range, the min element in the range, 

Each node stores
- Range (L, R)
- Data about the the range
- Left and right child
## Implementation
```python
import math

class SegmentTreeSum:
	def __init__(self, arr):
		# arr of (START, END, SUM)
		self.size = len(arr)
		self.arr = [None] * (2 ** ((len(arr) - 1).bit_length() + 1) - 1)
		def _build(i, start, end):
			if start == end:
				self.arr[i] = (start, end, arr[start])
				return self.arr[i][2]
			mid = (start + end) // 2
			left_sum = _build(self._left(i), start, mid)
			right_sum = _build(self._right(i), mid + 1, end)
			self.arr[i] = (start, end, left_sum + right_sum)
			return self.arr[i][2]
		_build(0, 0, len(arr) - 1)

	def __repr__(self):
		str_rep = f"SegmentTreeSum: {self.arr}\n"
		def dfs(i, d = 0):
			nonlocal str_rep
			if self._value(i) == None:
				return
			str_rep += f"{'    ' * d}[{self.arr[i][0]} - {self.arr[i][1]}] sum: {self.arr[i][2]}\n"
			dfs(self._left(i), d + 1)
			dfs(self._right(i), d + 1)
		dfs(0, 0)
		return str_rep

	def __str__(self):
		return repr(self)

	def _parent(self, i):
		if i == 0:
			return -1
		return i // 2

	def _left(self, i):
		return 2 * i + 1
	
	def _right(self, i):
		return 2 * i + 2

	def _value(self, i):
		if 0 <= i < len(self.arr):
			return self.arr[i]
		return None

	def build(arr):
		return SegmentTreeSum(arr)

	# find sum from start to end
	def query(self, start, end):
		splits = 0
		if not (0 <= start < self.size and 0 <= end < self.size):
			return None
		START, END, SUM = 0, 1, 2
		def search(curr, c_start, c_end):
			nonlocal splits
			curr_val = self._value(curr)
			if curr_val[START] == c_start and curr_val[END] == c_end:
				return curr_val[SUM]

			mid = (curr_val[START] + curr_val[END]) // 2
			if c_start > mid:
				return search(self._right(curr), c_start, c_end)
			elif c_end <= mid:
				return search(self._left(curr), c_start, c_end)
			else:
				print(f"      query split")
				splits += 1
				# c_start     mid     c_end
				return search(self._left(curr), c_start, mid) + \
					search(self._right(curr), mid + 1, c_end)
		res = search(0, start, end)
		print(f"    splits: {splits}")
		return res
	
	# update index with value
	def update(self, index, value):
		START, END, SUM = 0, 1, 2
		curr = 0
		while self._value(curr) != None:
			curr_val = self._value(curr)
			mid = (curr_val[START] + curr_val[END]) // 2
			if curr_val[START] == curr_val[END] == index:
				# found node, update value
				self.arr[curr] = (curr_val[START], curr_val[END], value)

				# propagate upwards O(log n)
				curr = self._parent(curr)
				while self._value(curr) != None:
					curr_val = self._value(curr)
					new_sum = 0
					right = self._value(self._left(curr))
					if right != None:
						new_sum += right[SUM]
					left = self._value(self._right(curr))
					if left != None:
						new_sum += left[SUM]
					self.arr[curr] = (curr_val[START], curr_val[END], new_sum)
					curr = self._parent(curr)
				return
			elif index > mid:
				curr = self._right(curr)
			else:
				curr = self._left(curr)
				
			

segment_tree = SegmentTreeSum([1, 5, 5, 3, 3, 2, 4, 4, 7, 8, 9, 1, 3, 4, 5, 16])
#							   0  1  2  3  4  5  6  7  8  9  10 11 12 13 14 15
#									    '-----' = 8
#				                  '--------------' = 22
#												    '-----------------' = 36

def test_query(start, end):
	print(f"  query [{start} - {end}] = {segment_tree.query(start, end)}")

def test_update(index, value):
	segment_tree.update(index, value)
	print(f"\n  update [{index}] = {value}")
	print(f"    new_tree: {segment_tree}")

print(f"init: {segment_tree}")
test_query(3, 5)
test_query(1, 6)
test_query(7, 13)

test_update(4, 13)

test_query(3, 5)
test_query(1, 6)
test_query(3, 13)
test_query(1, 32)
```
# Big O
Let $n$ = size of array
- Space
	- $O(n)$
- Time
	- Build
		- $O(n)$
			- Create $2n$ nodes, which is $O(n)$
	- Update
		- $O(\log n)$
	- Query Range
		- $O(\log n)$
		- Search for the cached ranges that make up `start` and `end`
			- Traverse depth of tree in worst case
		- At most 2 nodes are expanded (split into two calls) at every level
			- Therefore there are at most 2 branched searches, and the number of branches does not exponentially grow
# Flashcards
#flashcards/data-structures

Segment tree
?
- Binary tree that answers queries about a range of numbers within an array
	- **Ex.** Sum, min value, max value, or product of the specific range
- Big O
	- Space $\to O(n)$
		- Specifically we have $2n$ nodes in the tree
	- Time
		- Build $\to O(n)$
		- Update $\to O(\log n)$
		- Query Range $\to O(\log n)$
			- At most 2 nodes are expanded (split into two calls) at every level
- Implementation $\to$ can use array representation of binary tree
	- Build
		- Recursively split array into half
		- If `start == end`, then create node, return sum
	- Update
		- Find node with `start == end == index`, and overwrite that node's `value`
		- Then repeatedly visit parents and update their sums by adding left + right children
	- Query Range
		- If `start > mid`
			- Move right
		- Else if `end <= mid`
			- Move left
		- Else
			- Split into two searches from `start to mid`, and from `mid + 1 to end`
<!--SR:!2025-02-07,19,250-->