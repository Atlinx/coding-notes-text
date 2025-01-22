# The Skyline Problem
#problem #d-hard #line-sweep #heap
https://leetcode.com/problems/the-skyline-problem/description/

A city's **skyline** is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Given the locations and heights of all the buildings, return _the **skyline** formed by these buildings collectively_.

The geometric information of each building is given in the array `buildings` where `buildings[i] = [lefti, righti, heighti]`:

- `lefti` is the x coordinate of the left edge of the `ith` building.
- `righti` is the x coordinate of the right edge of the `ith` building.
- `heighti` is the height of the `ith` building.

You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height `0`.

The **skyline** should be represented as a list of "key points" **sorted by their x-coordinate** in the form `[[x1,y1],[x2,y2],...]`. Each key point is the left endpoint of some horizontal segment in the skyline except the last point in the list, which always has a y-coordinate `0` and is used to mark the skyline's termination where the rightmost building ends. Any ground between the leftmost and rightmost buildings should be part of the skyline's contour.

**Note:** There must be no consecutive horizontal lines of equal height in the output skyline. For instance, `[...,[2 3],[4 5],[7 5],[11 5],[12 7],...]` is not acceptable; the three lines of height 5 should be merged into one in the final output as such: `[...,[2 3],[4 5],[12 7],...]`

![[Pasted image 20250101113102.jpg]]
# Flashcards
#flashcards/problems 

The Skyline Problem
- A city's **skyline** is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Given the locations and heights of all the buildings, return _the **skyline** formed by these buildings collectively_.
- The geometric information of each building is given in the array `buildings` where `buildings[i] = [lefti, righti, heighti]`
- ![[Pasted image 20250101113102.jpg]]
?
- Line sweep
	- Big O
		- Time $\to O(n \log n)$
		- Space $\to O(n)$
	- Buildings are already sorted
	- `b_i` = next building to parse
	- `max_height_heap` = heap of ending edges, sorted by height
	- Iterate sorted edges by position, `e`
		- While `e` is ahead of `buildings[b_i].start`
			- Add next building `(height, end)` to `max_height_heap`
		- While `max_height_heap[0].end` is before `e`
			- Pop heap
		- If height of the top of `max_height_heap` changed, then add it to skyline
<!--SR:!2025-02-14,23,250-->