# Reconstruct Flight Path
#problem #d-hard #graph
https://neetcode.io/problems/reconstruct-flight-path

You are given a list of flight tickets `tickets` where `tickets[i] = [from_i, to_i]` represent the source airport and the destination airport.

Each `from_i` and `to_i` consists of three uppercase English letters.

Reconstruct the itinerary in order and return it.

All of the tickets belong to someone who originally departed from `"JFK"`. Your objective is to reconstruct the flight path that this person took, assuming each ticket was used exactly once.

If there are multiple valid flight paths, return the lexicographically smallest one.

- For example, the itinerary `["JFK", "SEA"]` has a smaller lexical order than `["JFK", "SFO"]`.

You may assume all the tickets form at least one valid flight path.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/e5ea2ea5-da22-4c22-a5c1-5840dab7fb00/public)

```java
Input: tickets = [["BUF","HOU"],["HOU","SEA"],["JFK","BUF"]]

Output: ["JFK","BUF","HOU","SEA"]
```

**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/9bfece1f-1fec-4618-4f95-31b2abcd3100/public)

```java
Input: tickets = [["HOU","JFK"],["SEA","JFK"],["JFK","SEA"],["JFK","HOU"]]

Output: ["JFK","HOU","JFK","SEA","JFK"]
```

Explanation: Another possible reconstruction is `["JFK","SEA","JFK","HOU","JFK"]` but it is lexicographically larger.

**Constraints:**

- `1 <= tickets.length <= 300`
- `from_i != to_i`
## Solution
```python
from collections import defaultdict

class Solution:
    # Review:
    #   Turns out the solution was the same as mine, except it didn't include dictionary manipulation 
    #   to track the number of edges left to know when to terminate the algorithm.
    #    
    #   Therefore, the timeout problem my solution was facing must be caused by 
    #   my dictionary manipulation and not the design of my algorithm
    def dfs_solution(self, tickets: List[List[str]]) -> List[str]:
        adj = {src: [] for src, dst in tickets}
        tickets.sort()
        for src, dst in tickets:
            adj[src].append(dst)

        res = ["JFK"]
        def dfs(src):
            if len(res) == len(tickets) + 1:
                return True
            if src not in adj:
                return False

            temp = list(adj[src])
            for i, v in enumerate(temp):
                adj[src].pop(i)
                res.append(v)
                if dfs(v): return True
                adj[src].insert(i, v)
                res.pop()
            return False
            
        dfs("JFK")
        return res


    # Solution using hierholzer
    def hierholzer_algo(self, tickets: List[List[str]]) -> List[str]:
        # build adj with edges sorted in reverse order, so each neighbor list can be popped in O(1) time 
        # to get next lexographically smallest edge
        adj = defaultdict(list)
        tickets.sort(reverse=True)
        for src, dst in tickets:
            adj[src].append(dst)

        print(f"adj: {adj}")

        res = []
        def dfs(src, d = 0):
            def dprint(m):
                print(f"{'  ' * d}{m}")
            dprint(f"[{src}]:")
            while adj[src]:
                dst = adj[src].pop()
                dprint(f"  pop {dst}, adj: {adj}")
                dfs(dst, d + 1)
            res.append(src)
            
        dfs('JFK')
        return res[::-1]


    # Review: need to use Hierholzer's algorithm
    #   Steps:
    #       DFS to get post-order path
    #       Reverse the post-order path to get the real path 
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        return self.hierholzer_algo(tickets)

```

### 1 - Greedy
I first attempted a greedy algorithm by first sorting the tickets, building the adjacency list, and then repeatedly using the next alphabetically smaller ticket from the current location. However this lead to unused tickets.
### 2 - DFS Exhaustive Search
My next attempt used DFS, which searched through all possible ticket combinations. However this is very slow, since an exhaustive DFS search creates an $O(V \cdot E)$ runtime.

**Proof of Runtime:**
- For each DFS call
	* We must check through every neighbor
		* In the worst case, a node is connected to every other node, which makes us check $(V - 1)$ neighbors
	* We remove the edge to that neighbor before recursively searching the neighbor $T(E - 1)$
- Using this information, we can build the following recurrence relation:
$$
\begin{align}
\begin{drcases}
T(E) &= (V - 1) \cdot T(E - 1) \\
&= (V - 1) \cdot T(E - 2) \\ \\
&\dots \\
&= (V - 1) \cdot 1 \\
\end{drcases} E
\end{align}
$$
- Since the recurrence relation above happens $E$ times, and every time we have $(V - 1)$ operations, we get the Big O runtime of
	- $O((V-1) \cdot E) = O(V \cdot E - E) = O(V \cdot E)$

### 3 - Hierholzer
The problem requires us to identify a path that uses all the tickets, and guarantees that such a path exists. Since tickets are edges in the graph, this means the graph has an [[Eulerian Graphs#^eulerian-path|Eulerian path]] — a path that traverses through every edge of the graph.

The algorithm to find a Eulerian path — given that such a path is guaranteed to exist — is called the [[Hierholzer Algorithm|Hierholzer's algorithm]]. 

This leads to a runtime of $O(V + E)$, since you only need to visit every node and edge once.


# Flashcards
#flashcards/problems 

Reconstruct flight path
- You are given a list of flight tickets `tickets` where `tickets[i] = [from_i, to_i]` represent the source airport and the destination airport.
- Each `from_i` and `to_i` consists of three uppercase English letters.
- Reconstruct the itinerary in order and return it.
- All of the tickets belong to someone who originally departed from `"JFK"`.
?
- [[Hierholzer Algorithm|Hierholzer's algorithm]]
	- Big O
		- Let $V$ = number of cities, $E$ = number of flight tickets
		- Time $\to O(V + E)$
		- Space $\to O(V)$
	- Because we're given an Eulerian graph
	- DFS down "JFK", and in post order, add elements to array
	- Reverse array and return
<!--SR:!2025-01-11,3,250-->