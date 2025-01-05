# Detect Squares
#problem #combinatorics 
https://leetcode.com/problems/detect-squares/description/

You are given a stream of points on the X-Y plane. Design an algorithm that:

- **Adds** new points from the stream into a data structure. **Duplicate** points are allowed and should be treated as different points.
- Given a query point, **counts** the number of ways to choose three points from the data structure such that the three points and the query point form an **axis-aligned square** with **positive area**.

An **axis-aligned square** is a square whose edges are all the same length and are either parallel or perpendicular to the x-axis and y-axis.

Implement the `DetectSquares` class:

- `DetectSquares()` Initializes the object with an empty data structure.
- `void add(int[] point)` Adds a new point `point = [x, y]` to the data structure.
- `int count(int[] point)` Counts the number of ways to form **axis-aligned squares** with point `point = [x, y]` as described above.
# Flashcards
#flashcards/problems 

Detect Squares
- You are given a stream of points on the X-Y plane. Design an algorithm that:
	- **Adds** new points from the stream into a data structure. **Duplicate** points are allowed and should be treated as different points.
	- Given a query point, **counts** the number of ways to choose three points from the data structure such that the three points and the query point form an **axis-aligned square** with **positive area**.
- An **axis-aligned square** is a square whose edges are all the same length and are either parallel or perpendicular to the x-axis and y-axis.
?
- Big O
	- Time $\to O(n)$
	- Space $\to O(n)$
- Store count dictionary of points $\to$ accounts for duplicates
	- `count_dict = [point]: count`
- When querying point `x, y`
	- Iterate all `count_dict` points `cx, cy`
		- If cur point is diagonal to query point and sides are equal
			- Calculate remaining square points
			- Query `count_dict` for counts
			- `combos += point_1_counts * point_2_counts * point_3_counts`