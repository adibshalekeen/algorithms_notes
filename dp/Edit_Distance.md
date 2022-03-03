There are two words, $w1$ and $w2$ figure out the minimum number of operations required to convert $w1$ to $w2$. You can do 3 operations on a word:
1) Insert a character
2) Delete a character
3) Replace a character

ex.

$w1$ = "almost"
$w2$ = "algomonster"

Output = 5

almost -> algmost
algmost -> algomost
algomost -> algomonst
algomonst -> algomonste
algomonste -> algomonster

Solution:

We can define a grid $dp[i][j]$ where each element in $dp$ represents the minimum edit distance required to convert the first $i$ chars in $w1$ to the first $j$ chars in $w2$ . 

In the table below, each element represents the minimum number of operations that can be performed to convert some substring $w1[0:i]$  to $w2[0:j]$ and vice-versa. If we know the edit distance for $w1[0: i -1]$ to $w2[0 : j - 1]$ then there are 3 possible operations we can perform to calculate the edit distance for $i$ and $j$.

1) $w1[i-1] == w2[j-1]$ , we dont need to do an operation. The edit distance $dp[i][j]$ would be the same as $dp[i - 1][j  - 1]$ 
2) We add a character to $w1$, $dp[i][j] = dp[i][j-1] + 1$
3) We delete a character from $w1$, $dp[i][j] = dp[i][j - 1] + 1$

We want to ensure that each element contains the smallest possible edit distance, we dont really care about the steps required to get to that minimum distance. We can define a recursive relation for when $w1[i] != w2[j]$:

$dp[i][j] = 1 + min(dp[i - 1][j] , dp[i][j-1], dp[i-1][j-1])$

Below is a sample table filled out for a portion of the example to help visualize the solution.

| --- | ---  | --- |  A  |  L  |  G  |  O  |
| --- | ---  | --- | --- | --- | --- | --- |
| --- | ---  |  0  |  1  |  2  |  3  |  4  |
| --- | 0    |  0  |  1  |  2  |  3  |  4  |
| A   | 1    |  1  |  0  |  1  |  2  |  3  |
| L   | 2    |  2  |  1  |  0  |  1  |  2  | 
| M   | 3    |  3  |  2  |  1  |  1  |  2  |
| O   | 4    |  4  |  3  |  2  |  2  |  1  |

Once the table is populated we can read $dp[len(w1)][len(w2)]$ for our answer.

```
def min_distance(w1: str, w2: str) -> int:
	# check if either string is empty
	if not w1 or not w2:
		return len(w1 + w2)

	n1 = len(w1)
	n2 = len(w2)
	dp = [[0 for _ in range(n2 + 1)] for _ in range(n1 + 1)]

	for i in range(n1 + 1):
		dp[i][0] = i

	for i in range(n2 + 1):
		dp[0][i] = i

	for i in range(n1 + 1):
		for j in range(n2 + 1):
			l = dp[i][j - 1] + 1
 			u = dp[i-1][j] + 1
			lu = dp[i-1][j-1]
			if w1[i-1] != w2[j-1]:
				lu += 1
			dp[i][j] = min([l, u, lu])

	return dp[n1][n2]
```

We can also perform a DFS to get the same result with less memory usage but increased time complexity
```
def min_distance(word1: str, word2: str) -> int:
        min_dist = 1000
        n1 = len(w1)
        n2 = len(w2)
        if not n1 or not n2:
            return n1 + n2
        def dfs(i, j, dist):
            nonlocal min_dist
            if i == n1 and j == n2:
                # w1 = w2
                min_dist = min(min_dist, dist)
                return
            if i == n1:
                # we have run out of characters in w1 and
                # need to do n2 - j inserts
                min_dist = min(min_dist, dist + n2 - j)
                return
            if j == n2:
                # we have reached the end of w2 and need
                # to do n1 - i deletions
                min_dist = min(min_dist, dist + n1 - i)
                return
            if w1[i] == w2[j]:
                dfs(i + 1, j + 1, dist)
            else:
                dfs(i + 1, j + 1, dist + 1)
                dfs(i + 1, j, dist + 1)
                dfs(i, j + 1, dist + 1)
        dfs(0, 0, 0)
        return min_dist
```

