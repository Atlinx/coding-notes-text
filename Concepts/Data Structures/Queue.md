# Queue
#data-structure 

A queue is a data structure that lets you store and retrieve elements in the order they arrive. This is similar to a line of customers waiting at a checkout.

This is commonly implemented using an cyclical array and two pointers representing the start index and end index of the queue.
## Implementation
```python
# PriorityQueue
class PriorityQueue:
	MIN_ARR_LEN = 8
	
	def __init__(self, size = 0):
		self.arr = [None] * max(size, self.MIN_ARR_LEN)
		self.start = 0
		self.end = 0

	def __len__(self):
		start, end = self.start, self.end
		if start > end:
			# Make sure end is always the largest
			start, end = end, start
		size = self.end - self.start
		if size == 0 and self.arr[start] != None:
			# If end and start are at the same point,
			# but we have items in the array, that means
			# the array is filled
			return len(self.arr)
		return size

	# Time: O(1)
	def enqueue(self, v):
		self.arr[self.end] = v
		self.end += 1
		if self.end >= len(self.arr):
			self.end = 0
		if self.end == self.start:
			# Resize up
			self._resize(len(self.arr) * 2)

	# Time: O(1)
	def dequeue(self):
		if len(self) == 0:
			return None
		value = self.arr[self.start]
		self.arr[self.start] = None
		self.start += 1
		if self.start >= len(self.arr):
			self.start = 0
		if len(self) < len(self.arr) // 2:
			# Resize down  
			self._resize(max(PriorityQueue.MIN_ARR_LEN, len(self.arr) // 2))
		return value

	# Time: O(n) (amortized O(1))
	def _resize(self, new_size):
		new_arr = [None] * new_size
		i = self.start
		for new_i in range(len(new_arr)):
			new_arr[new_i] = self.arr[i]
			i += 1
			if i >= len(self.arr):
				i = 0
			if i == self.end:
				break
		self.end = len(self)
		self.start = 0
		self.arr = new_arr

	def __repr__(self):
		arr_str = "["
		for i in range(len(self.arr)):
			arr_str += repr(self.arr[i])
			if self.start == i:
				arr_str += " (S)"
			if self.end == i:
				arr_str += " (E)"
			if i < len(self.arr) - 1:
				arr_str += ", "
		arr_str += "]"
		return f"Priority Queue: ({len(self)}) {arr_str}"

	def __str__(self):
		return self.__repr__()

pq = PriorityQueue()
for i in range(8):
	pq.enqueue(i)
print(f"add 1:\n  {pq}")
for i in range(8, 7):
	pq.enqueue(i)
print(f"add 2:\n  {pq}")
for i in range(7, 17):
	pq.enqueue(i)
print(f"add 3:\n  {pq}")
dequeued = []
for i in range(15):
	dequeued.append(pq.dequeue())
print(f"dequeued: {dequeued}")
print(f"  post-dequeue: {pq}")
```
## Big O
Let $n$ = number of elements in the queue
- Space
	- $O(n)$
- Time
	- Enqueue + Dequeue
		- $O(1)$
			- We insert and remove from a start and end pointer respectively
			- Even though we resize the array, because we double/half the size whenever we resize, in the long-run this resizing is negligible

# Flashcards
#flashcards/data-structures 

Queue
?
- Data structure to store and retrieve elements in the order they arrived
- Big O
	- Space $\to O(n)$
	- Time
		- Push/pop $\to O(1)$
<!--SR:!2025-01-06,4,270-->