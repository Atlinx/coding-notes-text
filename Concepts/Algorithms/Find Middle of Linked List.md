# Find Middle of Linked List
#algorithm 

A naïve approach is iterating once to get the size of the linked list, and then iterating exactly half, which would take $1.5n$ runtime.

A better approach is using a slow and fast pointer. Once the fast pointer hits the end of the linked list, the slow pointer should be exactly at the center of the linked list. This would take $1n$ runtime, since the longest distance we travel is the entire linked list, using the fast pointer.
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
	- Run