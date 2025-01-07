# Binary Search
#algorithm 

## Regular
```python
def binary_search(arr, target):
	lo = 0
	hi = len(arr) - 1
	while lo <= hi:
		mid = (lo + hi) // 2
		if arr[mid] > target:
			hi = mid - 1
		elif arr[mid] < target:
			lo = mid + 1
		else:
			return mid
	return -1

def test(arr, target):
	index = binary_search(arr, target)
	arr_str = ""
	ptr_str = ""
	for i in range(len(arr)):
		curr_ptr_str = ""
		if i == index:
			curr_ptr_str += "^"
		arr_str += str(arr[i]).ljust(4)
		ptr_str += curr_ptr_str.ljust(4)
	print(f"binary_search, target = {target}:")
	print(f"  found arr[{index}]: {arr[index] if 0 <= index < len(arr) else 'N/A'}")
	print(f"  arr:   [{arr_str}]")
	print(f"          {ptr_str}")

arr = [0, 0, 1, 1, 2, 4, 4, 4, 4, 4, 4, 5, 5, 6, 9]
test(arr, 0)
test(arr, 1)
test(arr, 3)
test(arr, 5)
test(arr, 4)
test(arr, 7)
test(arr, 9)
```
## Right-most
```python
def binary_search(arr, target):
	lo = 0
	hi = len(arr) - 1
	while lo <= hi:
		mid = (lo + hi) // 2
		print(f"lo: {lo} hi: {hi} mid: {mid} = {arr[mid]} vs. {target}")
		if arr[mid] > target:
			hi = mid - 1
		else:
			lo = mid + 1
	return hi

def test(arr, target):
	index = binary_search(arr, target)
	arr_str = ""
	ptr_str = ""
	for i in range(len(arr)):
		curr_ptr_str = ""
		if i == index:
			curr_ptr_str += "^"
		arr_str += str(arr[i]).ljust(4)
		ptr_str += curr_ptr_str.ljust(4)
	print(f"binary_search, target = {target}:")
	print(f"  found arr[{index}]: {arr[index] if 0 <= index < len(arr) else 'N/A'}")
	print(f"  arr:   [{arr_str}]")
	print(f"          {ptr_str}")

arr = [1, 1, 2, 4, 4, 4, 4, 4, 4, 5, 5, 6, 9]
test(arr, 0)
test(arr, 1)
test(arr, 3)
test(arr, 5)
test(arr, 4)
test(arr, 7)
test(arr, 9)
```

## Left-most
```python
def binary_search(arr, target):
	lo = 0
	hi = len(arr) - 1
	while lo <= hi:
		mid = (lo + hi) // 2
		if arr[mid] >= target:
			hi = mid - 1
		else:
			lo = mid + 1
	return lo

def test(arr, target):
	index = binary_search(arr, target)
	arr_str = ""
	ptr_str = ""
	for i in range(len(arr)):
		curr_ptr_str = ""
		if i == index:
			curr_ptr_str += "^"
		arr_str += str(arr[i]).ljust(4)
		ptr_str += curr_ptr_str.ljust(4)
	print(f"binary_search, target = {target}:")
	print(f"  found arr[{index}]: {arr[index] if 0 <= index < len(arr) else 'N/A'}")
	print(f"  arr:   [{arr_str}]")
	print(f"          {ptr_str}")

arr = [0, 0, 1, 1, 2, 4, 4, 4, 4, 4, 4, 5, 5, 6, 9]
test(arr, 0)
test(arr, 1)
test(arr, 3)
test(arr, 5)
test(arr, 4)
test(arr, 7)
test(arr, 9)
```
# Flashcards
#flashcards/algorithms 

Binary search: Find index
?
- `lo = 0, hi = max`
- While `lo <= hi`
	- `mid = (lo + hi) // 2`
	- If `arr[mid] > target`
		- `hi = mid - 1`
	- else if `arr[mid] < target`
		- `lo = hi + 1`
	- else
		- Return `mid`

Binary search: Find left-most index
- `lo = 0, hi = max`
- While `lo <= hi`
	- `mid = (lo + hi) // 2`
	- If `arr[mid] >= target`
		- `hi = mid - 1`
	- else
		- `lo = mid + 1`
- Return `lo`

Binary search: Find right-most index
- `lo = 0, hi = max`
- While `lo <= hi`
	- `mid = (lo + hi) // 2`
	- If `arr[mid] > target`
		- `hi = mid - 1`
	- else
		- `lo = mid + 1`
- Return `hi`