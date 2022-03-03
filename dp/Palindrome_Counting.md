Count the number of palindromes that exist in a particular string. Duplicates are allowed as long as they cover distinct intervals of the string. Empty strings do not count towards the total.

ex.

Input = abab
Output = 6

The following intervals are palindroms: [0, 0], [1, 1], [2, 2], [3, 3], [0, 2], [1, 3]

Solution:

For each entry in the solution matrix $dp[i][j]$ let the entry represent whether the string starting at index $i$ and ending at index $j$ is a palindrome. If $dp[i-1][j-1]$ tells us whether $s[i:j]$ is a palindrome then $dp[i][j] = dp[i+1][j-1]\ and\ s[i] == s[j]$. 


```
def palindrome_counting(s: str) -> int:
	dp = [[False for _ in s] for _ in s]
	n = len(s)
	
	# Initialize all substrings of length 1 to be a palindrome
	# Set all substrings where i > j to be true
	for i in range(n):
		for j in range(0, i):
			dp[i][j] = True
	for i in range(n):
		print(dp[i])
	# Iterate through based on the length of substring and starting index
	# its 0 indexed so 0 is reserved for length 1 strings
	# Remember each individual char is technically a palindrome
	count = len(s)
	for strlen in range(1, n):
		for i in range(n - strlen):
			j = i + strlen
			dp[i][j] = dp[i + 1][j - 1] and s[i] == s[j]
			if dp[i][j]:
				count += 1
	return count
	
```
