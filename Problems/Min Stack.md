# Min Stack
#problem 
https://leetcode.com/problems/min-stack/description/

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

# Flashcards
#flashcards/problems 

Min Stack
- Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
- Implement the `MinStack` class:
	- `MinStack()` initializes the stack object.
	- `void push(int val)` pushes the element `val` onto the stack.
	- `void pop()` removes the element on the top of the stack.
	- `int top()` gets the top element of the stack.
	- `int getMin()` retrieves the minimum element in the stack.
- You must implement a solution with `O(1)` time complexity for each function.
?
- Two stacks
	- `stack` = stack that contains values
	- `min_stack` = stack that tracks minimum at a given point
	- When pushing, `new_min = min(min_stack[-1], new_num)`
	- When popping, pop both stacks
<!--SR:!2025-01-26,8,250-->