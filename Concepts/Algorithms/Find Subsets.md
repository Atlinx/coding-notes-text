# Find Subsets
#algorithm 
## Distinct Elements
To find all possible subsets of an `array` of distinct numbers, we iterate through the array and ask whether we want to include the number or not.
### Implementation
```python
arr = [1, 3, 5]
def subsets(arr):
	res = []
	temp = []
	def dfs(i):
		if i >= len(arr):
			return
		
		# include
		temp.append(arr[i])
		res.append(list(temp))
		dfs(i + 1)
		temp.pop()
		
		# exclude
		dfs(i + 1)
	dfs(0)
	return res
print(f"arr: {arr}\nsubsets: {subsets(arr)}")
```
![[Pasted image 20250103145203.png]]
## Non-distinct Elements
To find all possible subsets of an `array` of non-distinct numbers, we first sort the numbers. Then, we can iterate through each number and ask if we want to include it or not in the subset. We must also factor in the choice of skipping over a number entirely, which requires moving past the contiguous chunk of the same number
### Implementation
```python
arr = [1, 4, 3, 1, 1,]
def subsets_nondistinct(arr):
	res = []
	arr.sort()
	temp = []
	def dfs(i):
		if i >= len(arr):
			return
		
		# include
		temp.append(arr[i])
		res.append(list(temp))
		dfs(i + 1)
		temp.pop()
		
		# exclude
		i += 1
		while i < len(arr) and arr[i] == arr[i - 1]:
			i += 1
		dfs(i)
	dfs(0)
	return res
print(f"arr: {arr}\nsubsets: {subsets_nondistinct(arr)}")
```
## Big O
Let $n$ = number of elements in `array`
- Space
	- $O(n)$
		- Excluding the space needed to store the final list of subsets 
- Time
	- $O(n *2^n)$
		- By asking questions of include or excluding a number, we create a binary tree. This tree would have $2^n$ nodes, because each number adds an entire row to the tree, and doubles the amount of options
		- For each option, we also traverse $[i, n]$ 
# Flashcards
#flashcards/algorithms 

Find subsets
?
- Big O
	- Time $\to O(n * 2^n)$
		- $2^n$ total possibilities, and we create a copy of each possibility before adding it to the final list
	- Space $\to O(n)$ if excluding space needed to store final list of subsets
- DFS backtracking with pointer `i` $\to$ current element we're considering
	- Do we include current element or not? `append` then `pop` from `temp` array
	- Non-distinct `array`
		- Must sort the `array` before hand
		- Not including the an element means moving `i` until we hit a different element
<!--SR:!2025-01-21,9,250-->