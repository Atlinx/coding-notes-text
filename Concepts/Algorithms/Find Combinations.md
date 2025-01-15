# Find Combinations
#algorithm 
Combination problems are framed as choosing $k$ elements from an array of $n$ total elements. This is similar to [[Find Subsets|finding subsets]] of an array, except we're only interested in subsets of size $k$.
## Subsets
We can use the same [[Find Subsets]] algorithm but modify it to have a limit for how many items it can add to the array. Once we hit that limit, we copy the `temp` array into our results. 
### Implementation
```python
arr = [1, 3, 5]

# Subsets approach
# Time:  O(k * 2^n)
# Space: O(n)
def combos(arr, k):
	if k > len(arr):
		# cannot choose more elements than there are in array
		return
	
	arr.sort()
	res = []
	temp = []
	def dfs(i):
		if len(temp) == k:
			res.append(list(temp))
			return
		if i >= len(arr):
			return
		# include
		temp.append(arr[i])
		dfs(i + 1)
		temp.pop()
		# exclude
		i += 1
		while i < len(arr) and arr[i] == arr[i - 1]:
			i += 1
		dfs(i)
	dfs(0)
	return res

def test_combos(arr, k):
	print(f"arr: {arr}, k: {k}\n  combos: {combos(arr, k)}")

test_combos(arr, 1)
test_combos(arr, 2)
test_combos(arr, 3)
test_combos(arr, 4)
```
![[Pasted image 20250103145104.png]]
### Big O
Let $n$ = number of elements in `arr` and $k$ = number of elements to choose 
- Space
	- $O(n)$ 
		- Excluding the space needed to store the final list of subsets
- Time
	- $O(k * 2^n)$
		- Like [[Find Subsets|finding subsets]], except we only need a depth of `k` elements
## Combinatorics (Distinct)
If the `arr` elements are distinct, we can do better than the [[Find Subsets]] algorithm by iterating over `k` empty slots and choosing an element to fill the slot. After choosing an element to fit the `kth` slot, we're left with `n - 1` options for the next slot, and `k - 1` remaining slots.
### Implementation
```python
from collections import Counter

arr = [1, 3, 5, 7]

# Combinatorics approach
# Time:  O(k * n choose k)
# Space: O(n + k)
def combos(arr, k):
	if k > len(arr):
		# cannot choose more elements than there are in array
		return

	arr.sort()
	res = []
	temp = []
	def dfs(i):
		if len(temp) == k:
			res.append(list(temp))
			return
		
		# choose an element to insert into current slot
		# we still need to fill (k - len(temp) - 1) slots in temp
		#     note the -1 is for the element we add in the current iteration
		#     that is temp.append(arr[j])
		for j in range(i, len(arr) - (k - len(temp) - 1)):
			temp.append(arr[j])
			dfs(j + 1)
			temp.pop()
	dfs(0)
	return res

def test_combos(arr, k):
	print(f"arr: {arr}, k: {k}\n  combos: {combos(arr, k)}")

test_combos(arr, 1)
test_combos(arr, 2)
test_combos(arr, 3)
test_combos(arr, 4)
```
![[Pasted image 20250103150002.png]]
### Big O
Let $n$ = number of elements in `arr` and $k$ = number of elements to choose
- Space
	- $O(k)$ 
		- Excluding the space needed to store the final list of subsets
- Time
	- $O(k * n C k)$
		- Notice how this tree has depth of $k$, and the leaf nodes are always the final combinations 
			- We can say it takes $k$ steps to get to each result
		- Since the number of combinations is $nCk$ (n choose k), we can say the time complexity is $O(k * nCk)$
		- Note that $O(nCk) < O(2^n)$
# Flashcards
#flashcards/algorithms 

Find combinations - 2 approaches
?
- Two approaches
	- Subset
	- Combinatoric
<!--SR:!2025-01-19,7,250-->

Find combinations: Subset approach
?
- Big O
	- Time $\to O(k * 2^n)$
	- Space $\to O(n)$
- Implementation
	- Sorted `arr`
	- DFS backtracking, index `i` in `arr`
		- If `len(temp) == k`, add `temp` to results
		- Try including `arr[i]`, `dfs(i + 1)`
		- Try excluding `arr[i]` and move past duplicates, then `dfs(i)`
<!--SR:!2025-01-19,7,250-->

Find combinations: Combinatoric (distinct) approach
?
- Big O
	- Time $\to O(k * nCk)$
	- Space $\to O(n)$
- Only works for `arr` of distinct elements
- Implementation
	- DFS backtracking, index `i` in `arr`
		- If `len(temp) == k`, add `temp` to results
		- Choose number `j` from `[i, n]`
			- `dfs(j + 1)`
<!--SR:!2025-01-16,2,230-->