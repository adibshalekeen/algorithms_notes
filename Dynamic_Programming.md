-> Break down a complicated problem into smaller overlapping subproblems

-> Problem can be:
	   - Divided into subproblems, the optimal solution to the problem can be constructed from the optimal solutions of the subproblems (optimal substructure)
	 - Subproblems must overlap

1. Optimal Substructure

Consider the problem of finding the shortest driving path between SF and SD.
	- The highway goes through LA so we can break this up into two subproblems
	- shortest_path(SF, SD) = shortest_path(SF, LA) + shortest_path(LA, SD)
	- The optimal solution to the problem is the optimal solution of the subproblems

If you compare this to the problem of finding the cheapest flight from SF to SD; even though you can get a flight from SF to LA, LA to SD it will cost more because airliners charge for layovers. So the optimal solution to the subproblem isn't the optimal solution to the main problem.

2. Overlapping Subproblems

Whenever we have repeated computation, like with calculating fibonacci sequence

![[fib.png]]

There are two different approaches to DP, top-down and bottom-up.

#### Top-Down DP
Split a large problem recursively and solve smaller subproblems, combine them into the larger solution

1) Draw the tree
2) Identify states
	- What state do we need to know when we've reached a solution?
	- What state do we need to decide which child nodes should be visited?
3) DFS + Memoization

#### Bottom-Up DP
Solve the subproblems first and use the solutions to find solutions to bigger subproblems (usually tabular)

```
def fib(n: Int):
	dp = [0, 1]
	for i in range(2, n + 1):
		dp.append(dp[i - 1] + dp[i - 2])
	return dp[-1]
```

You slowly build up a table of values from some known initial conditions. 

#### Recurrence Relation
Identifying the recurrence relation is the key to dynamic programming. You need to figure out how the larger problem is related to the smaller subproblems. In the case of the fibonacci sequence the recurrance relation is $dp[i] = dp[i-1]  + dp[i-2]$. This isn't always a mathematical relation, and can be a logical one too (for example when working with strings).

#### Top Down vs Bottom Up
-> Top down pros
	- The order of computing the subproblems doesn't matter
	- Easier to reason for partition problems (how many ways are there to ..., splitting a string into...)

-> Buttom up pros 
	- Easier to calculate runtime
	- No recursion so less likely to incurr a system stack overflow (or maximum recursion depth)

#### Greedy Algorithm vs Dynamic Programming
A greedy algorithm is when we always want to pick the best answer. For dynamic programming we may not ncessarily want the best answer for every state to get the best answer for the overall problem due to some restriction in the problem statement.


#### Divide and Conquer vs Dynamic Programming
Divide and conquer algorithms use subproblems, but the subproblems don't overlap. Merge sort is an example of this.

#### Dynamic Programming Patterns
Generally dynamic programming is a good candiadate for a solution of they have any of the following signs:
	- Asking for the maximum/longest, minimal/shortest value/cost/profit you can get from doing operations on a sequence
	- Greedy algorithms sometimes give the wrong solution, so you need to consider each subproblem for an optimal solution
	- Asking for how many ways to do something (DFS + memoization)
	- Asking to martition a string/array into sub-sequences so a certain condition is met
	- Asking for the optimal way to play a game

1) Interval
	Problems involving finding the subproblem defined on an interval $dp[i][j]$
	[Palindrome Counting](dp/Palindrome_Counting)
	[Palindrome Partitioning](dp/Palindrome_Partitioning)
	[Combine String](dp/Combine_String)
	
2) Two Sequences
	Problems with two sequences in the problem statement. $dp[i][j]$ represents the max/min/best value for the first sequence ending in index i and second sequence ending in index j
	[Edit Distance](dp/Edit_Distance)
	[Delete String](dp/Delete_String)
	[Longest Common Subsequence](dp/Longest_Common_Subsequence)

3) Sequence
	Finding the max / min of some sequence with rules that define the recurrence relation between elements
	[House Robber](dp/House_Robber)
	[Coin Change](dp/Coin_Change)
