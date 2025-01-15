# Line Sweep
#algorithm 

This is an algorithm that  answer queries about which interval a point is in. This can be an interval with the biggest size, or by some other metric.
## Implementation
```python
import heapq

# Line Sweep Algorithm
#
# let n = number of intervals, q = number of queries
# Time:  O(q + n log n)
# Space: O(n)
def line_sweep_min(intervals: list[tuple[int, int, int]], \
			   queries: list[int]) -> list[int]:
	# intervals -> list of intervals (start, end, value), duplicates allowed 
	# queries   -> list of queries   (end, value),        duplicates allowed
	
	I_START, I_END, I_VALUE = 0, 1, 2 # interval indicies:   (start, end, value)
	H_VALUE, H_END = 0, 1             # heap entry indicies: (end, value)
	
	intervals.sort()       # sort intervals by starting position
	i = 0                  # track last processed interval
	interval_min_heap = [] # heap of intervals we're currently processing
	ans_dict = {}          # map query to answer
	for q in sorted(set(queries)): # iterate unique queries sorted by position
		# add intervals
		while i < len(intervals) and intervals[i][I_START] <= q:
			heapq.heappush(interval_min_heap, \
				(intervals[i][I_VALUE], intervals[i][I_END]))
			i += 1
		# ensure top of heap is an interval that hasn't ended yet
		while interval_min_heap and interval_min_heap[0][H_END] < q:
			heapq.heappop(interval_min_heap)
		# top of heap is our answer
		ans_dict[q] = interval_min_heap[0][H_VALUE] if interval_min_heap else -1

	# answer each query
	res = []
	for q in queries:
		res.append(ans_dict[q])
	return res


res = line_sweep_min([(1, 2, 3), (2, 3, 1), (2, 5, 4), (2, 5, 3)], \
		  [2, 4, 5, 6, 1])
print(f"res: {res}")
#           input: [2, 4, 5, 6,  1]
# expected output: [1, 3, 3, -1, 3]
```
## Big O
Let $n$ = number of intervals and $q$ = number of distinct queries
- Space
	- $O(n)$
		- Store all intervals in interval heap
- Time
	- $O(q + n \log n)$
		- Every interval is added to the heap ($\log n$) and removed from the heap ($\log n$) once, therefore $O(n \cdot 2 \log n) = O(n \log n)$
		- Assuming we have sorted intervals and queries, otherwise we'd need
			- $O(q \log q + n \log n)$ to account for the sorting of the queries and intervals
# Flashcards
#flashcards/algorithms 

Line sweep algorithm
?
- Big O
	- Let $n$ = number of intervals and $q$ = number of distinct queries
	- Time $\to O(q + n \log n)$
		- Every interval is added to the heap ($\log n$) and removed from the heap ($\log n$) once, therefore $O(n \cdot 2 \log n) = O(n \log n)$
	- Space $\to O(n)$
		- Store all intervals in interval heap
- `intervals: list[(start, end, value)]` $\to$ sorted intervals
- `queries: list[point]` $\to$ sorted distinct queries
- `i = 0` $\to$ last processed interval
- `int_end_heap: list[(end, value)]` $\to$ heap of interval endpoints
-  Iterate through sorted `queries`, `q`
	- While `intervals[i].start <= q` $\to$ add new intervals
		- `int_end_heap.push((intverals[i].value, intverals[i].end))`, `i += 1`
	- While `int_end_heap[0].end > q` $\to$ ensure top of heap is an interval that hasn't ended
		- `int_end_heap.pop()`
	- `int_end_heap[0].value` = answer for query `q`
<!--SR:!2025-01-26,12,230-->