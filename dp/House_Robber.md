A robber is planning to rob houses along a street. Each house has a particular amount of treasure inside. If you rob some house at index $i$ you cant rob $i-1$ or $i+1$. What is the maximum amount of wealth the robber can steal from a row of houses?

ex.

Input = [2, 7, 9, 3, 1]
Output = 12

You can rob 2, 9 and 1 to get 12

Solution:

Define $dp[i]$ and the maximum amount of money that can be robbed at index $i$. If you robbed $i-1$ you cant rob $i$, if you robbed $i-2$ you cant rob $i-1$ but you can rob $i$. To figure out which is optimal, we can define $dp[i]$ with a recurrence relation.

$dp[i]\ =\ max(dp[i-2] + values[i], dp[i-1])$

```
def house_robber(values: List[int]) -> int:
	# Our base cases are 0, 1 so we need to check they exist
	if len(values) == 0:
		return 0
	if len(values) == 1:
		return values[0]
	if len(values) == 2:
		return max(values[0], values[1])
	dp = [0 for _ in values]
	dp[0] = values[0]
	dp[1] = values[1]
	for i in range(2, len(values)):
		dp[i] = max(dp[i-2] + values[i], dp[i-1])
	return dp[-1]
```
