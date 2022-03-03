Given an array of integers sorted in ascending order find two numbers that add up to a given target. Return the indices of the two numbers in ascending order. Assume the elements in the array are unique and there is only one solution. Do this in O(n) time and O(1) space.

ex.

Input = [2, 3, 5, 8, 11, 15], 5

Output: [0, 1]

2 + 3 = 5

Solution:

We can initialize two pointers, $left = 0$ $right = len(n) - 1$. If the sum of these entries is greater than our target then we can decrement the right pointer, if its smaller then we can increment the left pointer. If it matches we return [left, right].

```
def two_sum(nums: List[int], target: int) -> List[int]
	n = len(nums)
	left = 0
	right = n - 1
	while left < right:
		nsum = nums[left] + nums[right]
		if nsum > target:
			right -= 1
		if nsum < target:
			left += 1
		if nsum == target:
			return[left, right]
	return None
```