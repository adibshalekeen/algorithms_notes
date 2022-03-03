Given 3 strings $s1$, $s2$, $s3$ return whether or not its possible to make the 3rd string by merging the 2 other strings. The relative order of the characters in $s1$ and $s2$ must be maintained and every character in $s1$ and $s2$ must be used.

ex.

$s1$ = abe
$s2$ = cdf
$s3$ = abcdef

Output = True

You can make $s3$ by doing $ab + cd + e+ f = abcdef$

Solution:

We cant use a greedy solution because the letters in $s1$ and $s2$ have to be used sequentially. For example, if you have the following case $s1 = abe$, $s2= abef$, $s3 = abeafbe$ , you need to use $abe$ from $s2$ first, or you wont be able to use the $f$ later on in the string. For a dynamic programming approach, we can define our matrix $dp[i][j]$ as a boolean matrix. Each element $(i, j)$ is True if we can generate $s3[0: i + j + 1]$ from the first $i$ letters in $s1$ and the first $j$ letters in s2. The recurrance relation here is:

$dp[i][j] = (dp[i-1][j]\ and\ s1[i - 1] == s3[i + j - 1])\ or\ (dp[i][j-1] and s2[j - 1] == s3[i + j - 1])$

If $dp[len(s1)][len(s2)] = True$ then we can create $s3$ by mixing $s1$ and $s2$ .

Note that since $i$ and $j$ represent lengths, $dp$ is 1-indexed, not 0-indexed.

```
def combine_string(s1: str, s2: str, s3: str) -> bool:
	# We can do some preliminary checks on the string lengths
	# to see if this is even possible
	n1 = len(s1)
	n2 = len(s2)
	n3 = len(s3)
	if n1 + n2 != n3 or not s1 or not s2 or not s3:
		return False
	
	dp = [[False for _ in range(n2 + 1)] for _ in range(n1 + 1)]

	for i in range(1, n1 + 1):
		if s1[i - 1] == s3[i - 1]:
			dp[i][0] = True
		else:
			break

	for j in range(1, n2 + 1):
		if s2[i - 1] == s3[i - 1]:
			dp[0][i] = True
		else:
			break

	# First characters of either s1 or s2 has to match
	# first character of s3 or we can early exit
	if not dp[0][1] and not dp[1][0]:
		return False

	dp[0][0] = True
	for i in range(1, n1 + 1):
		for j in range(1, n2 + 1):
			if dp[i-1][j] and s1[i - 1] == s3[i + j - 1]:
				dp[i][j] = True
			if dp[i][j-1] and s2[j - 1] == s3[i + j - 1]:
				dp[i][j] = True
	return dp[n1 - 1][n2 - 1]

```