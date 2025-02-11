# Longest Special Path
#problem #d-hard #dfs #sliding-window 
https://leetcode.com/problems/longest-special-path/description/

You are given an undirected tree rooted at node `0` with `n` nodes numbered from `0` to `n - 1`, represented by a 2D array `edges` of length `n - 1`, where `edges[i] = [ui, vi, lengthi]` indicates an edge between nodes `ui` and `vi` with length `lengthi`. You are also given an integer array `nums`, where `nums[i]` represents the value at node `i`.

A **special path** is defined as a **downward** path from an ancestor node to a descendant node such that all the values of the nodes in that path are **unique**.

**Note** that a path may start and end at the same node.

Return an array `result` of size 2, where `result[0]` is the **length** of the **longest** special path, and `result[1]` is the **minimum** number of nodes in all _possible_ **longest** special paths.

**Example 1:**
> **Input:** `edges = [[0,1,2],[1,2,3],[1,3,5],[1,4,4],[2,5,6]], nums = [2,1,2,1,3,1]`
> **Output:** `[6,2]`
> **Explanation:**
> In the image below, nodes are colored by their corresponding values in `nums`
> ![[Pasted image 20250119153359.png]]
# Flashcards
#flashcards/problems 

Longest Special Path
- You are given an undirected tree rooted at node `0` with `n` nodes numbered from `0` to `n - 1`, represented by a 2D array `edges` of length `n - 1`, where `edges[i] = [ui, vi, lengthi]` indicates an edge between nodes `ui` and `vi` with length `lengthi`. You are also given an integer array `nums`, where `nums[i]` represents the value at node `i`.
- A **special path** is defined as a **downward** path from an ancestor node to a descendant node such that all the values of the nodes in that path are **unique**.
- **Note** that a path may start and end at the same node.
- Return an array `result` of size 2, where `result[0]` is the **length** of the **longest** special path, and `result[1]` is the **minimum** number of nodes in all _possible_ **longest** special paths.
- **Example 1:**
	- **Input:** `edges = [[0,1,2],[1,2,3],[1,3,5],[1,4,4],[2,5,6]], nums = [2,1,2,1,3,1]`
	- **Output:** `[6,2]`
	- **Explanation:**
		- In the image below, nodes are colored by their corresponding values in `nums`
		- ![[Pasted image 20250119153359.png]]
?
- DFS + Sliding window
	- Big O
		- Time $\to O(E + V)$
		- Space $\to O(E + V + V) = O(E + V)$
			- Adjacency list representation of graph + deepest DFS search possible (longest stack possible)
	- `adj` = Convert edges to adjacency matrix representation of graph
	- `dist = {}`
		- DFS to build `dist` mapping of length from every node to root
		- Enables $O(1)$ length calculation between two nodes along the same downward path
	- DFS to find special paths, `v = current node`
		- Shared vars
			- `path_start` = starting point of unique path (left ptr of sliding window
			- `path` = stack of nodes along path
			- `last_seen` = dictionary mapping node to last seen index
		- If `nums[v]` in `last_seen` and `last_seen[nums]` is ahead of `path_start`
			- `path_start = last_seen[nums] + 1`
		- Add `v` to `path` and `last_seen`
		- DFS children of `v`
		- Undo any changes to `path_start, path` and `last_seen`
<!--SR:!2025-02-11,1,250-->