# Find Permutations
#algorithm 
Permutation problems are framed as how many ways can we reorder $n$ elements. This means `[1, 2, 3]` $\neq$ `[2, 1, 3]`.
## All Permutations
How many ways can we reorder $n$ elements? This means `[1, 2, 3]` $\neq$ `[2, 1, 3]`.
### Implementation
```python
arr = [1, 3, 5, 7]

def permutes(arr):
	res = [[]]
	for elem in arr: # O(n)
		for i in range(len(res) - 1, -1, -1): # O(n)
			res_len = len(res[i])
			for j in range(res_len + 1): # O(n)
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
## Choose k
How many ways can we choose $k$ elements from an array of $n$ elements when order matters? This means `[1, 2, 3]` $\neq$ `[2, 1, 3]`.
### Implementation
```python
arr = [1, 3, 5, 7]

def permutes(arr):
	res = [[]]
	for elem in arr:
		for i in range(len(res) - 1, -1, -1):
			res_len = len(res[i])
			for j in range(res_len + 1):
				copy = list(res[i])
				copy.insert(j, elem)
				res.append(copy)
	return res

def test_permutes(arr):
	print(f"arr: {arr},\n  permutes: {permutes(arr)}")

test_permutes(arr)
```
# Flashcards
#flashcards/algorithms 

Find permutations
?
- Big O
	- Time $\to O(n^2 \cdot n!)$
	- Space $\to O\left( \sum\limits_{i = 0}^n i! \cdot {n \choose i} \right)$
		- Not important to memorize exact formula, just reasoning
		- We find permutations of every possible subset of `arr`
- Only works for `arr` of distinct elements