# Indexed Binary Heap
#graph #data-structure 

This is a [[Binary Heap|binary heap]] that lets you efficiently update the priority of values. This is done by using a dictionary to map the entries in the heap to specific indices in the array, which enables us to sink/swim the entry if we ever update itself value.
## Implementation
```python
class MaxIndexHeap:
	def __init__(self, key_value_dict = None):
		self.arr = [] # [i] = (key, value)
		self.map = {} # [key]: i
		if key_value_dict != None:
			self.arr = list(key_value_dict.items())
			for i in range(len(self.arr)):
				self.map[self.arr[i][0]] = i
		self.heapify()

	def __len__(self):
		return len(self.arr)
	
	def __repr__(self):
		return f"MinHeap: {self.arr} {self.map}"
	
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
				
				i_key, si_key = self.arr[i][0], self.arr[si][0]
				self.arr[i], self.arr[si] = self.arr[si], self.arr[i]
				self.map[i_key], self.map[si_key] = si, i
				i = si
			else:
				# We are at the right place
				return

	def _swim(self, i):
		pi = i // 2
		while self._ahead_of(i, pi):
			# swap with parent
			i_key, pi_key = self.arr[i][0], self.arr[pi][0]
			self.arr[i], self.arr[pi] = self.arr[pi], self.arr[i]
			self.map[i_key], self.map[pi_key] = pi, i
			i = pi
			pi = i // 2

	def _ahead_of_value(self, a, b):
		return a > b

	def _ahead_of(self, a, b):
		return self._ahead_of_value(self.arr[a][1], self.arr[b][1])

	# Time: O(n)
	def heapify(self):
		for i in reversed(range(len(self.arr) // 2)):
			self._sink(i)

	# Time: O(1)
	def has(self, key) -> bool:
		return key in self.map

	# Time: O(1)
	def get(self, key):
		if key in self.map:
			return self.arr[self.map[key]][1]
		return None

	# Time: O(log n)
	#
	# Return
	# - True if created for first time
	# - False if key was updated
	def push(self, key, value):
		if key in self.map:
			self.update(key, value)
			return False
		self.arr.append((key, value))
		self.map[key] = len(self.arr) - 1
		self._swim(len(self.arr) - 1)
		return True
	
	# Time: O(log n)
	#
	# Return largest value
	def pop(self):
		if len(self.arr) == 0:
			return None
		key_value = self.arr[0]
		del self.map[key_value[0]]
		if len(self.arr) > 1:
			last_key_value = self.arr.pop()
			self.arr[0] = last_key_value
			self.map[last_key_value[0]] = 0
			self._sink(0)
		else:
			self.arr.pop()
		return key_value

	# Time: O(log n)
	#
	# Return
	# - True if key exists and was updated
	# - False if key does not exist
	def update(self, key, value):
		if not key in self.map:
			return False
		i = self.map[key]
		old_value = self.arr[i][1]
		self.arr[i] = (key, value)
		if self._ahead_of_value(value, old_value):
			self._swim(i)
		else:
			self._sink(i)
		return True

class MinIndexHeap(MaxIndexHeap):
	def _ahead_of_value(self, a, b):
		return a < b

heap = MinIndexHeap({
	0: 30,
	1: 48, 
	2: 7,
	3: 3,
	4: 24,
	5: 19,
	6: 2,
	7: 3,
	8: 8,
	9: 3,
	10: 14,
	11: 16
})
print(f"heap 1: {heap}")
popped = []
for i in range(5):
	popped.append(heap.pop()[1])
print(f"popped 2: {popped}")
print(f"heap 2: {heap}")
for i in range(6):
	heap.push(i, i)
print(f"heap 3: {heap}")
popped.clear()
for i in range(5):
	popped.append(heap.pop()[1])
print(f"popped 4: {popped}")
print(f"heap 4: {heap}")
```
# Big O
Let $n$ = number of elements in the heap
- Space
	- $O(n)$
- Time
	- Heapify
		- $O(n)$
			- Same as [[Binary Heap|binary heap]]
	- Push
		- $O(\log n)$
			- Same as [[Binary Heap|binary heap]]
	- Pop
		- $O(\log n)$
			- Same as [[Binary Heap|binary heap]]
	- Update
		- $O(\log n)$
			- We either sink or swim the node whenever we update, therefore we traverse at most the entire height of the binary tree
# Flashcards
#flashcards/data-structures 

Indexed binary heap
?
- Binary heap where you can update the value of an entry
- Insert key, value into heap
	- Implemented using dictionary that maps key to node index in array
- Big O
	- Space $\to O(n)$
	- Time
		- Heapify $\to O(n)$
		- Push $\to O(\log n)$
		- Pop $\to O(\log n)$
		- Update $\to O(\log n)$
<!--SR:!2025-04-05,55,250-->