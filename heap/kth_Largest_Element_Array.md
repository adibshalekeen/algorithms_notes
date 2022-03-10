Given an integer array nums, and an integer k, return the k-th largest element in the array.

ex.

Input: nums = [3, 2, 1, 5, 6, 4] k = 2

Output: 5

The sortest list is [1, 2, 3, 4, 5, 6], the second largest element is 5

Solution:

We can sort $nums$ and return the k-th largest element, if we use a heap to sort it then we can execute in $O(n + klog\ n)$ time by running heapify on the input array (which runs in $O(n)$) and then poping the first $len(nums) - k$ items.

```
import heapq
def findKthLargest(nums: List[int], k: int) -> int:
	heapq.heapify(nums)
	for _ in range(len(nums) - k):
		heapq.heappop(nums)
	return nums[0]
```