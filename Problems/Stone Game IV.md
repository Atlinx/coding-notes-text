# Stone Game IV
#problem #d-hard #dynamic-programming #game-theory
https://leetcode.com/problems/stone-game-iv/description/ 

Alice and Bob take turns playing a game, with Alice starting first.

Initially, there are `n` stones in a pile. On each player's turn, that player makes a _move_ consisting of removing **any** non-zero **square number** of stones in the pile.

Also, if a player cannot make a move, he/she loses the game.

Given a positive integer `n`, return `true` if and only if Alice wins the game otherwise return `false`, assuming both players play optimally.

# Flashcards
#flashcards/problems 

Stone Game IV
- Alice and Bob take turns playing a game, with Alice starting first.
- Initially, there are `n` stones in a pile. On each player's turn, that player makes a _move_ consisting of removing **any** non-zero **square number** of stones in the pile.
- Also, if a player cannot make a move, he/she loses the game.
- Given a positive integer `n`, return `true` if and only if Alice wins the game otherwise return `false`, assuming both players play optimally.
?
- 1-D Dynamic programming
	- Big O
		- Time $\to O(n \cdot \sqrt{ n })$
			- There are $\sqrt{ n }$ perfect squares under $n$
			- For each case ($n$ total), we want to iterate over the number of perfect squares ($\sqrt{ n }$ in worst case given $n$ stones)
		- Space $\to O(n)$
	- Subproblem $\to$ At a given n, does current player win the game?
	- Note that we don't need to have another axis for the player (Alice/Bob), because the game is symmetric
<!--SR:!2025-02-10,21,250-->