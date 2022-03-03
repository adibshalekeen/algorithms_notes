Given a sorted list of numbers, remove the duplicates and return the new length of the array. This has to be done in-place with no extra memory usage.

ex.

Input = [0, 0, 1, 1, 1, 2, 2]

Output = 3, [0, 1, 2, x x x x]

[0, 1, 2] are the only unique values in the input array, they are sorted and placed at the beginning of the array. Then we return 3 to signify the valid length of the output array.

Solution:

We need two pointers, one marking the index in the output array and one running through all of the duplicate values. Since all of the numbers are sorted, we know that once we have observed all of the duplicate values of some number while traversing left -> right, we won't encounter that value again.

 L  R
[0, 0, 1, 1, 1, 2, 2]
     L  R
[0, 1, 1, 1, 1, 2, 2]
         L         R
[0, 1, 2, 1, 1, 2, 2]
         L                 R
[0, 1, 2, 1, 1, 2, 2]
```
def remove_duplicates(nums: List[int]) -> int:
	right = 1
	for left in range(0, len(nums)):
		while right < len(nums) and nums[left] == nums[right]:
			right += 1
		if right >= len(nums):
			break
		nums[left + 1] = nums[right]
	return left + 1
```