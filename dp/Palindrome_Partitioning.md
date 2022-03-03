Given some string $s$ partition $s$ such that every substring in the partition is a palindrome. Similar to [Palindrome Counting](dp/Palindrome_Counting). 


ex.

Input: aab
Output: 2

Possible partitions: ["a", "a", "b"], ["aa", "b"], ["aab"], ["a", "ab"]. Only two of these partitions only contain substrings that are all palindromes.

Solution:

For this solution, since we are partitioning $s$ we should use a top-down approach so we can visualize the problem better. Take a look at this tree outlining how we can partition the base string.

![[palindrome_partitioning_1.png]]


The yellow boxes are intermediate states, the red boxes are terminal states and the red boxes with green outlines are valid terminal states (all elements are palindromes). So given some slice of the string $s$ from indices $i$ to $j$, if the slice is a palindrome, add it to our current state tracker and repeat the process for the remainder of the string $s[j + 1:]$. If we use up all of the characters in $s$, ($i == j == len(s)$) then we can increment our number of valid partitions by 1. We can use the solution to the [Palindrome Counting](dp/Palindrome_Counting) problem to preprocess whether a substring is a valid palindrome

```
def partition(s: str) -> int:
	# First part is exactly the same as palindrome counting
	dp = [[False for _ in s] for _ in s]
	n = len(s)
	for i in range(n):
		for j in range(0, i + 1):
			dp[i][j] = True

	for strlen in range(1, n):
		for i in range(n - strlen):
			j = i + strlen
			dp[i][j] = dp[i + 1][j - 1] and s[i] == s[j]

	# Now dp contains an array that tells us if some substring s[i:j] is a valid
	# palindrome
	def recursive_partition(start_index: int,
							valid_partitions: List[List[str]],
							cur_path: List[str]):
		if start_index == n:
			valid_partitions.append(cur_path[:])
			return

		# For each substr starting at i, of length strlen
		# run this recursive function to further partition the remaining
		# substring
		for strlen in range(0, n - start_index):
			j = start_index + strlen
			if dp[start_index][j]:
				recursive_partition(j + 1,
									valid_partitions,
									cur_path + [s[start_index:j + 1]])
	valid_partitions = []
	recursive_partition(0, valid_partitions, [])
	return len(valid_partitions)
```

This will work, but since we only need the number of valid partitions and not the values of the partitions we can optimize a bit further. 

We need to define a secondary array $dp2$ of length $len(s)$ where each element $i$ represents the number of valid partitions using only the first $i$ letters in $s$ . If substring $s[i : j]$ is found to be a palindrome then the number of valid partitions using the first $j$ letters can be incremented by the number of valid partitions using the first $i - 1$ letters. If we start at some non-zero index $j$ within $s$ and work our way backwards to generate substrings, then this gives us a recurrence relation where $dp[j] += dp[i - 1]$ if $s[i:j]$ is a palindrome.

```
def partition(s: str) -> int:
	# First part is exactly the same as palindrome counting
	dp = [[False for _ in s] for _ in s]
	n = len(s)
	for i in range(n):
		for j in range(0, i + 1):
			dp[i][j] = True

	for strlen in range(1, n):
		for i in range(n - strlen):
			j = i + strlen
			dp[i][j] = dp[i + 1][j - 1] and s[i] == s[j]

	# Now dp contains an array that tells us if some substring s[i:j] is a valid
	# palindrome
	dp2 = [0 for _ in s]
	dp2[0] = 1
	for i in range(1, n):
		for j in range(i, 0, -1):
			if dp[j][i]:
				dp2[i] += dp2[j - 1]
		if dp[0][i]:
			dp2[i] += 1
	return dp2[-1]

```