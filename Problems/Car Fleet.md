# Car Fleet
#problem #d-medium #iteration
https://leetcode.com/problems/car-fleet/description/

There are `n` cars traveling to the same destination on a one-lane highway.

You are given two arrays of integers `position` and `speed`, both of length `n`.

- `position[i]` is the position of the `ith car` (in miles)
- `speed[i]` is the speed of the `ith` car (in miles per hour)

The **destination** is at position `target` miles.

A car can **not** pass another car ahead of it. It can only catch up to another car and then drive at the same speed as the car ahead of it.

A **car fleet** is a non-empty set of cars driving at the same position and same speed. A single car is also considered a car fleet.

If a car catches up to a car fleet the moment the fleet reaches the destination, then the car is considered to be part of the fleet.

Return the number of **different car fleets** that will arrive at the destination.
# Flashcards
#flashcards/problems 

Car Fleet
- There are `n` cars traveling to the same destination on a one-lane highway.
- You are given two arrays of integers `position` and `speed`, both of length `n`.
	- `position[i]` is the position of the `ith car` (in miles)
	- `speed[i]` is the speed of the `ith` car (in miles per hour)
- The **destination** is at position `target` miles.
- A car can **not** pass another car ahead of it. It can only catch up to another car and then drive at the same speed as the car ahead of it.
- A **car fleet** is a non-empty set of cars driving at the same position and same speed. A single car is also considered a car fleet.
- If a car catches up to a car fleet the moment the fleet reaches the destination, then the car is considered to be part of the fleet.
- Return the number of **different car fleets** that will arrive at the destination.
?
- Iteration
	- Big O
		- Time $\to O(n \log n)$
		- Space $\to O(1)$
	- Time car arrives at destination determines car groupings
	- `fleets` = 1
	- `prev_time` = time of last car
	- Iterate over cars in reversed sorted order by position
		- Calculate time each car will arrive using distance formula
			- `d = r * t`
			- `t = d / r`
			- `t = (dest - start) / speed`
		- If  `curr_time > prev_time`, then update `prev_time` and add a new fleet
<!--SR:!2025-02-24,15,210-->