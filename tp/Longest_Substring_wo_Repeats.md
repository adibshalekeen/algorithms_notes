Given a string, find the length of the longest substring without repeating characters.


ex.

Input = "`abccabcabcc`"

Output = 3

The longest substring with no repeating characters are "abc" or "cab"


Solution:

We can define two pointers $left$ and $right$. $left$ will always point towards the left most character in the substring, and $right$ will point towards the right most. We can increase the length of the substring by incrementing $right$. Each time we increase the size of the substring we need to keep track of all of the unique characters that have been observed so far in a set. If there is a collision in the set then we increment $left$ until the collision is resolved.

```
def longest_substring_without_repeating_characters(s: str) -> int:
	left = 0
	right = 0
	chars = set()
	max_len = 0
	while right < len(s):
		if s[right] in chars:
			chars.remove(s[left])
			left += 1
		else:
			chars.add(s[right])
			right += 1
		max_len = max(max_len, right - left)
	return max_len

```