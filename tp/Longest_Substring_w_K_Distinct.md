Given a string $s$ and integer $k$ return the length of the longest substring of $s$ that contains at most $k$ distinct characters


ex.

$s$ = "eceba", k = 2

Output = 3

"ece" is the longest substring with at most k distinct characters

Solution:

This is similar to [Longest Substring without Repeating Characters](tp/Longest_Substring_wo_Repeats). But instead of tracking repeating characters we are tracking unique ones. For every unique character observed we need to note down the position of the character. When we observe more than $k$ unique characters, we need to figure out the position of the left most character and increment our $left$ pointer to the right of that position. If there are duplicate values of a character observed, we always want to keep track of the right most position of the character, otherwise our substring will still contain a copy of it when we update $left$.

```
def length_of_longest_substring_k_distinct(s: str, k: int) -> int:
	left, right = 0, 0
	char_map = {}
	max_len = 0
	while right < len(s):
		char_map[s[right]] = right
		right += 1
		if len(char_map) > k:
			delete_idx = min(char_map.values())
			del char_map[s[delete_idx]]
			left = delete_idx + 1
		max_len = max(max_len, right - left)
	return max_len
```