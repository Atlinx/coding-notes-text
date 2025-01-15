# Prefix Sum
#algorithm 

- Prefix sums are an array that counts the cumulative total of another array.
- Suffix sums are the cumulative total an arr but starting from the back.

**Ex.**
```
arr    = [0,  1,  3,  2,  7,  4 ]
prefix = [0,  1,  4,  6,  13, 17]
suffix = [17, 17, 16, 13, 11, 4 ]
```

## Uses
- When you need to find range query to the left of a node and the range query to the right of a node
	- Use prefix + suffix sum
	- `curr[i] = prefix[i] + suffix[i]`
- When you need to find the sum of a range of numbers
	- `sum[l, r] = prefix[r] - prefix[l - 1]`
- **NOTE:**
	- Always prefer prefix sums over segment trees, since prefix sums provide $O(1)$ range queries while segment trees provide $O(\log n)$ range queries.
	- If you need to update the array, then use segments trees because they take $O(\log n)$ to update, compared to $O(n)$ for prefix sums.


# Flashcards
#flashcards/algorithms 

Prefix sum
?
- Prefix sums are an array that counts the cumulative total of another array.
- Suffix sums are the cumulative total an arr but starting from the back.
- Uses
	- When you need to find range query to the left of a node and the range query to the right of a node
		- Use prefix + suffix sum
		- `curr[i] = prefix[i] + suffix[i]`
	- When you need to find the sum of a range of numbers
		- `sum[l, r] = prefix[r] - prefix[l - 1]`
	- **NOTE:**
		- Always prefer prefix sums over segment trees, since prefix sums provide $O(1)$ range queries while segment trees provide $O(\log n)$ range queries.
		- If you need to update the array, then use segments trees because they take $O(\log n)$ to update, compared to $O(n)$ for prefix sums.
<!--SR:!2025-01-17,3,250-->