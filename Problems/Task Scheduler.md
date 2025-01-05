# Task Scheduler
#problem #heap 
https://leetcode.com/problems/task-scheduler/

You are given an array of CPU `tasks`, each labeled with a letter from A to Z, and a number `n`. Each CPU interval can be idle or allow the completion of one task. Tasks can be completed in any order, but there's a constraint: there has to be a gap of **at least** `n` intervals between two tasks with the same label.

Return the **minimum** number of CPU intervals required to complete all tasks.

**Example 1:**
> **Input:** `tasks = ["A","A","A","B","B","B"], n = 2`
> 
> **Output:** 8
> 
> **Explanation:** A possible sequence is: A -> B -> idle -> A -> B -> idle -> A -> B.
> 
> After completing task A, you must wait two intervals before doing A again. The same applies to task B. In the 3rd interval, neither A nor B can be done, so you idle. By the 4th interval, you can do A again as 2 intervals have passed.

# Flashcards
#flashcards/problems 

Task Scheduler
- You are given an array of CPU `tasks`, each labeled with a letter from A to Z, and a number `n`.
- Each CPU interval can be idle or allow the completion of one task. Tasks can be completed in any order, but there's a constraint: there has to be a gap of **at least** `n` intervals between two tasks with the same label.
- Return the **minimum** number of CPU intervals required to complete all tasks.
?
- Simulate cycles
	- Big O
		- Time $\to O(n)$
			- Note we don't include $\log n$ from heap b/c max heap size is fixed to 26 (all 26 letters from alphabet)
		- Space $\to$ $O(1)$
			- Note we don't include $n$ from heap b/c max heap size is fixed to 26 (all 26 letters from alphabet)
	- `task_heap` $\to$ Max heap of count of every task
		- Stored as `(-count)`
	- `cooldown_queue` $\to$ Queue of tasks waiting on cooldown
		- Stored as `(next_cycle, count)`
	- While `task_heap or cooldown_queue`
		- Move ready tasks from `cooldown_queue` to `task_heap`
		- If `task_heap`, then pop from `task_heap`
		- Increment cycle