Given 2 strings, determine the minimum cost required to delete characters from either string to make them equal. Each character has a cost that must be incurred to delete it from either string. The costs are associated to each character from a-z (lowercase only).

ex.

costs = [1, 2, 3, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

s1 = abb
s2 = bba

Output = 2

Deleting a from both strings incurrs a cost of 2 (1 for each string) and results in bb

Solution:

Let $dp[i][j]$ represent the minimum cost required to make $s1[:i]$ equal to $s2[:j]$. For any $dp[i][j]$, if $s1[i] = s2[j]$ then the cost is just equal to $dp[i-1][j-1]$ because no deletion is necessary. If they aren't equal then we can either delete $s1[i-1]$ or $s2[j-1]$, but we want to do so in a way that minimizes the over-all cost, so we have to consider the cost to make $s1[:i-1] = s2[:j]$ and $s1[:i] = s2[:j-1]$ as well.

Here is a sample table converting the test case.
            b b a
       0  1  2 3
   0  0  2 4 5
a 1   1  3 4 4
b 2  3  1 3 4
b 3  5  3 1 2

if $s1[i-1] != s2[j-1]$ then
	$dp[i][j] = min(dp[i-1][j] + cost[s1[i-1]], dp[i][j-1] + cost[s2[j-1]])$
else
	$dp[i][j] = dp[i-1][j-1]$

```
def delete_string(costs: List[int], s1: str, s2: str):
	n1 = len(s1)
	n2 = len(s2)
	dp = [[0 for _ in range(n2 + 1)] for _ in range(n1 + 1)]

	minchar = ord('a')
	for i in range(1, n1 + 1):
		dp[i][0] = dp[i-1][0] + costs[ord(s1[i-1]) - minchar]

	for i in range(1, n2 + 1):
		dp[0][i] = dp[0][i-1] + costs[ord(s2[i-1]) - minchar]

	for i in range(1, n1 + 1):
		for j in range(1, n2 + 1):
			if s1[i-1] == s2[j-1]:
				dp[i][j] = dp[i-1][j-1]
			else:
				s1_cost = costs[ord(s1[i-1]) - minchar]
				s2_cost = costs[ord(s2[j-1]) - minchar]
				dp[i][j] = min(dp[i-1][j] + s1_cost, dp[i][j-1] + s2_cost)
	return dp[n1][n2]
```