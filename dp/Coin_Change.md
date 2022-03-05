You are given coins of various denominations and a target amount. Write a function to compute the lowst number of coins needed to make up the target amount. If you cant make the target then return -1. You can use an infinite number of each coin denomination.

ex.

Input = [1, 2, 5], 11
Output = 3

11 = 5 + 5 + 1 (3 total coins)

Solution:

If you create an array $dp[i]$ where each element contains the minimum number of coins required to sum up to $i$. We can figure out how to sum up to $i$ by checking $i-coin$ where $coin$ is any of the available denominations, once we find the minimum value among all the possibilities, we can increment it by 1 to get the minimum number of coins required to total to $i$. Then the recurrence relation for $dp[i]$ can be defined as:

$dp[i]\ =\ min(dp[i\ -\ coin], dp[i])$ for coin in coins + 1

```
def coin_change(coins: List[int], target: int) -> int:
	if len(coins) == 0:
		return -1
	if target == 0:
		return 0
	dp = [float('inf') for _ in range(target + 1)]

	# Assume target > max(coins)
	for coin in coins:
		if coin <= target:
			dp[coin] = 1

	for i in range(target + 1):
		for coin in coins:
			if i - coin >- 0:
				dp[i] = min(dp[i - coin], dp[i]) + 1
	if dp[target] < float('inf'):
		return dp[target]
	return -1
	
```