Given two strings $w1$ and $w2$ return the length of their longest common subsequence. A subsequence is any string generated from the original string with some characters deleted but the relative ordering of the remaining characters must match the ordering in the original string.

ex.

$w1$ = "abcde"
$w2$ = "ace"

Output = 3

The longest subsequence is ace, which has length 3

Solution:

Bottom-Up Approach

We can define $dp[i][j]$ to contain the length of the maximum length subsequence between some string $w1[:i-1]$ and $w2[:j-1]$. For any given length tuple $(i,j)$ the maximum sub length can be built based on $dp[i-1][j],\ dp[i][j-1],\ and\ dp[i-1][j-1]$.

If $w1[i-1] == w2[j-1]$ then
	$dp[i][j] = dp[i-1][j-1] + 1$
else
	$dp[i][j] = max(dp[i-1][j], dp[i][j-1])$

Here is a sample table for the example above
                 a  c  e
	      0  1  2  3
	0    0  0  0  0 0   
a    1     0  1    1    1
b    2    0  1    1     1
c    3    0  1    2    2 
d    4    0  1    2    2
e    5    0  1    2    3

```
def maximum_common_subsequence(w1: str, w2: str):
	n1 = len(w1)
	n2 = len(w2)
	dp = [[0 for _ in range(n2 + 1)] for _ in range(n1 + 1)]

	for i in range(1, n1 + 1):
		for j in range(1, n2 + 1):
			if w1[i - 1] == w2[j - 1]:
				dp[i][j] = dp[i-1][j-1] + 1
			else:
				dp[i][j] = max(dp[i-1][j], dp[i][j-1])
	return dp[n1][n2]
```

Top-Down Approach

You can do a similar process by considering the matching process in a tree structure. For any given index $i, j$ we can either choose to match $w1[i]$ to $w2[j]$, increment $i$ or increment $j$. Each tuple $(i,j)$ represents a node on a tree. If we have a match while processing a node, we can increment the length of the maximum possible sub sequence by 1 and execute the process on the next node at $(i+1, j+1)$. Otherwise we will have to check $(i + 1, j)$ and $(i, j +1)$.

```
def maximum_common_subsequence(w1: str, w2: str):
	n1, n2 = len(w1), len(w2)
	def dfs(i, j):
		if i >= n1 or j >= n2:
			return 0
		max_len = 0
		if w1[i] == w2[j]:
			max_len = max(max_len, dfs(i + 1, j + 1) + 1)
		max_len = max(max_len, dfs(i+1, j))
		max_len = max(max_len, dfs(i, j + 1))
		return max_len
	return dfs(0, 0)
		
```