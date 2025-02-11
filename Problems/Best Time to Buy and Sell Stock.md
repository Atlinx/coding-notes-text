# Best Time to Buy and Sell Stock
#problem #d-easy 
https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

# Flashcards
#flashcards/problems 

Best Time to Buy and Sell Stock
- You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
- You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.
- Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.
?
- Sliding window
	- Iterate `prices` (Right pointer)
		- Track smallest price you've seen so far (Left pointer)
		- Calculate current `profit = price - smallest_price`
			- Keeping running `max_profit = max(max_profit, profit)`
	- If `max_profit < 0`, then don't buy any stocks at all
<!--SR:!2025-03-05,23,250-->