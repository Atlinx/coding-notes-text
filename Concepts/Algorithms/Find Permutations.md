# Find Permutations
#algorithm 
Permutation problems are framed as how many ways can we reorder $n$ elements. This means `[1, 2, 3]` $\neq$ `[2, 1, 3]`.
## All Subset Permutations
Find all permutations, including subsets.
### Implementation
```python
arr = [1, 3, 5, 7]

def permutes(arr):
	res = [[]]
	for elem in arr: # O(n)
		for i in range(len(res) - 1, -1, -1): # O(n!) -> scales by # perms
			perm_len = len(res[i])
			for j in range(perm_len + 1): # O(n)
				copy = list(res[i])
				copy.insert(j, elem)
				res.append(copy)
	return res

def test_permutes(arr):
	print(f"arr: {arr},\n  permutes: {permutes(arr)}")

test_permutes(arr)
```
### Big O
Let $n$ = number of elements in `arr` and $k$ = number of elements to choose
- Space
	- $O\left( \sum\limits_{i = 0}^n i! \cdot {n \choose i} \right)$
		- $i!$ = the number of permutations for a given subset of size $n$
		- We find permutation of each possible subset, and we have $n$ choose $i$ ways to pick a subset of size $i$ 
- Time
	- $O(n^2 \cdot n!)$
		- We iterate over each element $O(n)$
		- We iterate over the existing permutations ($n!$ permutations total) $O(n!)$
		- For each element, we iterate through positions to insert $O(n)$
## All Permutations
How many ways can we choose $k$ elements from an array of $n$ elements when order matters? This means `[1, 2, 3]` $\neq$ `[2, 1, 3]`.
### Implementation
```python
arr = [1, 3, 5, 7]

def permutes(arr):
	res = [[]]
	for elem in arr: # O(n)
		new_res = []
		for i in range(len(res) - 1, -1, -1): # O(n!) -> scales by # perms
			perm = res.pop()
			for j in range(len(perm) + 1): # O(n)
				copy = list(perm)          # O(n)
				copy.insert(j, elem)
				new_res.append(copy)
		res = new_res
	return res

def test_permutes(arr):
	print(f"arr: {arr},\n  permutes: {permutes(arr)}")

test_permutes(arr)
```
### Big O
Let $n$ = number of elements in `arr` and $k$ = number of elements to choose
- Space
	- $O(n!)$
		- There are $n!$ permutations for a set of $n$ numbers 
- Time
	- $O(n^2 \cdot n!)$
		- We iterate over each element $O(n)$
		- We iterate over the existing permutations ($n!$ permutations total) $O(n!)$
		- For each element, we iterate through positions to insert $O(n)$
# Flashcards
#flashcards/algorithms 

Find permutations
?
- Only works for `arr` of distinct elements
- Two types
	- All subsets perms $\to$ read from `res`, make copy, insert, and append
		- Big O
			- Time $\to O(n^3 \cdot \sum\limits_{i = 0}^n i! \cdot {n \choose i})$
				- We iterate each element in the array ($n$)
				- We iterate over existing results ($\sum\limits_{i = 0}^n i! \cdot {n \choose i}$)
				- We iterate over possible slots ($n$)
				- We make a copy of the current result ($n$), insert the element into the corresponding slot, and insert the copy back into the results array
			- Space $\to O\left( \sum\limits_{i = 0}^n i! \cdot {n \choose i} \right)$
				- Not important to memorize exact formula, just reasoning
				- We find permutations of every possible subset of `arr`
	- All perms $\to$ remove from `res`, make copy, insert, and append
		- Big O
			- Time $\to O(n^3 \cdot n!)$
				- We iterate each element in the array ($n$)
				- We iterate over existing results ($n!$)
				- We iterate over possible slots ($n$)
				- We make a copy of the current result ($n$), insert the element into the corresponding slot, and insert the copy back into the results array
			- Space $\to O\left( \sum\limits_{i = 0}^n i! \cdot {n \choose i} \right)$
				- We find permutations of every possible subset of `arr`
- Implementation
	- `res = [[]]`
	- Iterate `arr`, `a`
		- Iterate `res`, `r`
			- Iterate insertion slots in `r`
				- Make copy of `r` and insert `a` into slot
<!--SR:!2025-02-13,2,230-->