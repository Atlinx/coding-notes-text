# Tortoise/Hare Algorithm
#algorithm #graph 

This is an algorithm to detect cycles in a linked list with $O(1)$ space complexity, using a pointer that moves 2 nodes (fast/hare pointer) at a time and a pointer that moves 1 node (slow/tortoise pointer) at a time.

## Implementation
```python
#          0  1  2  3  4  5  6  7
A_llist = [2, 7,-1,-1, 3, 7, 4, 4]
B_llist = [1,-1, 3, 4, 5, 3, 5, 6]

def detect_cycle(llist) -> bool:
	if len(llist) == 0:
		return False
	
	visited = [False] * len(llist)
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

print(f"cycle in A? {detect_cycle(A_llist)}")
print(f"cycle in B? {detect_cycle(B_llist)}")
```

![[Pasted image 20250102164619.png]]
- Linked list A has no directed cycles
- Linked list B has a directed cycle, indicated in orange
## Big O
Let $V$ = number of vertices in graph, $E$ = number of edges in graph
- Space
	- $O(1)$
- Time
	- $O(n)$
		- We visit every node
# Flashcards
#flashcards/algorithms 

Tortoise Hare Algorithm
?
- Big O
	- Time $\to O(n)$
	- Space $\to O(1)$
- Finds a cycle in a linked list with $O(1)$ space
- Implementation
	- Start `slow` on first node, and `fast` on second node
	- While pointers haven't hit dead-ends,
		- Move `slow` forward 1 node
		- Move `fast` forward 2 nodes
		- If `slow == fast`, then cycle occurred
<!--SR:!2025-02-13,24,250-->