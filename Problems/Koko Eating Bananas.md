# Koko Eating Bananas
#problem #d-medium #binary-search 
https://leetcode.com/problems/koko-eating-bananas/description/

Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.
# Flashcards
#flashcards/problems 

Koko Eating Bananas
- Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.
- Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.
- Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.
- Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.
?
- Big O
	- Time $\to O(n \log m)$
		- Let $m$ = largest pile size, $n =$ number of piles
		- We search from 1 to $m$, therefore binary search takes $\log m$
	- Space $\to O(1)$
- Binary search solution space (left-most to achieve min)
	- `lo = 1`
		- Slowest Koko can eat is 1 banana/hour
	- `hi = max(piles)`
		- Fastest Koko can eat is the largest pile/hour
	- Use `mid` speed to simulate eating each pile and find how many hours it will take $O(n)$
	- If `mid_hours > hours` $\to$ increase speed
	- Else $\to$ decrease speed
	- Return `lo`
<!--SR:!2025-01-21,3,250-->