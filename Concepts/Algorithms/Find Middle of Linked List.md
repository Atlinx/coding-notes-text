# Find Middle of Linked List
#algorithm 

A naïve approach is iterating once to get the size of the linked list, and then iterating exactly half, which would take $1.5n$ runtime.

A better approach is using a slow and fast pointer. Once the fast pointer hits the end of the linked list, the slow pointer should be exactly at the center of the linked list. This would take $1n$ runtime, since the longest distance we travel is the entire linked list, using the fast pointer.
## Implementation
```python
#          0  1  2  3  4  5  6  7
A_llist = [1, 2, 3, 4, 5, 6, 7, 0]

def detect_cycle(llist) -> bool:
	if len(llist) == 0:
		return False
	
	for v in range(len(llist)):
		if not visited[v]:
			slow = v
			fast = llist[v]
			if fast < 0:
				# hit dead end
				continue
			
			# while fast and slow pointers haven't hit a deadend
			while fast >= 0 and slow >= 0:
				visited[fast] = True
				visited[slow] = True
				if fast == slow:
					# if fast and slow pointer overlap, then there's a cycle
					return True

				if llist[fast] < 0:
					# hit dead end
					break
				fast = llist[llist[fast]]
				slow = llist[slow]
	# no pointers ever overlapped, no cycle
	return False

detect_cycle(A_llist)
```
## Big O
Let $n$ = number of elements in the linked list
- Space
	- $O(1)$
- Time
	- $O(n)$
# Flashcards
#flashcards/algorithms 

Find middle of linked list
?
- Big O
	- Time $\to O(n)$
	- Space $\to O()$
- Tortoise Hare algorithm
	- Start `slow` and `fast` pointers at root
	- Move `slow` pointer 1 node at a time
	- Move `fast` pointer 2 nodes at a time
	- If `slow == fast`
<!--SR:!2025-01-10,3,250-->