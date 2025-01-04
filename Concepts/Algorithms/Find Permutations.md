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
	- $O(n^n)$
- Time
	- $O(nn)$
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
	- 
- Only works for `arr` of distinct elements