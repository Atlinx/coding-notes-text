# Two Heap
#algorithm 

Using a min and max heap together, we can identify the median of a stream of data.
- `min_heap` $\to$ stores top half
	- Returns element closest to middle, from top
- `max_heap` $\to$ stores bottom half
	- Returns element closest to middle, from bottom
## Implementation
```python
import heapq

class MedianStream():
	def __init__(self):
		self.min_heap = []
		self.max_heap = []

	def size(self):
		return len(self.min_heap) + len(self.max_heap)

	# O(log n) 
	def add_value(self, v):
		heapq.heappush(self.min_heap, v)
		if len(self.min_heap) > self.size() // 2:
			res = heapq.heappop(self.min_heap)
			heapq.heappush(self.max_heap, -res)

	# O(1)
	def get_median(self):
		if self.size() == 0:
			return 0
		if self.size() % 2 == 0:
			min_med = -self.max_heap[0]
			max_med = self.min_heap[0]
			return (min_med + max_med) / 2
		return -self.max_heap[0]

stream = [2, 6, 10, 3, 7, 1, 9, 8, 4, 5]
med_stream = MedianStream()
for i, s in enumerate(stream):
	med_stream.add_value(s)
	print(f"ins: {f"{s},":<3}   med: {med_stream.get_median():<5}   curr_stream: {sorted(stream[:i+1])}")
```
## Big O
Let $n$ = number of elements in stream
- Space
	- $O(n)$
- Time
	- Add Value
		- $O(\log n)$
	- Get Median
		- $O(1)$

# Flashcards
#flashcards/algorithms 

Two heap algorithm
?
- Algorithm for finding median from elements in a stream of data
- Use two heaps
	- `min_heap` $\to$ stores upper half of elements, head is largest median
	- `max_heap` $\to$ stores lower half of elements, head is smallest median
- Insert data
	- Insert data into `min_heap`
	- If `len(min_heap) > total size / 2`
		- Pop from `min_heap` and insert into `max_heap`
- Get median
	- Return `max_heap[0]` if size is odd, otherwise return average of `max_heap[0]` and `min_heap[0]`