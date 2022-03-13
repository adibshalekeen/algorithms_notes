Given an array $arr$ of $N$ integers (where $N$ is very large), for each index $i$ determine the number of contiguous subarrays that fulfill the following conditions:

1) The value at index $i$ is the maximum value in the sub array
2) The subarray begins or ends at index $i$

ex.

Input: [3, 4, 1, 6, 2]
Output: [1, 3, 1, 5, 1]

For index 0 - [3]
	index 1 - [3, 4], [4], [4, 1]
	index 2 - [1]
	index 3 - [3, 4, 1, 6], [4, 1, 6], [1, 6], [6], [6, 2]
	index 4 - [2]

Solution:

For any index $i$ within $arr$ there are two types of subarrays, the arrays starting at $i$ and the arrays ending at $i$. $arr[i]$ needs to be the maximum value within the subarray, so the maximum length of the subarray starting at $i$ is determined by the position $k$ of some value $arr[k]$ such that $k > i\ and\ arr[k] > arr[i]$. Similarly, the maximum length of the subarray ending at $i$ is determined by the position $j$ of some value $arr[j]$ such that $j < i\ and\ arr[j] > arr[i]$. 

If we iterate through $arr$ and build subarrays starting at $i$ while iterating from left->right, then we will cover all the subarrays that start at $i$, lets say this is $L[i]$ where $L[i]$ is there number of valid subarrays ending at index $i$ for which property 1) is valid. Similarly, if we iterate from right->left, we can define $R[i]$ as the number of valid subarrays ending at $i$.  Our total number of valid subarrays then becomes $res[i] = R[i] + L[i] - 1$. We subtract 1 here because the subarray $[arr[i]]$ is valid during the left->right traversal and the right->left traversal, so we would double count it.

We still need to figure out how to find the valid subarrays given that we are only processing one direction at a time. Lets use the left->right iteration as an example. The largest subarray that we can make spans from $[j, i]$ where $j < i\ and\ arr[j] > arr[i]$ . The number of subarrays that we can make from this span of indices is equal to $i - j$, since we can create one subarray for each continuous slice in the span, $[[j:j+1], [j:j+2], ... [j:i-1], [j:i]]$. So we iterate until $j + x = i$, where $x$ is the number of subarrays, $x = i - j$. You can do a similar exercise for the right->left iteration.

So while iterating left-> right, and given the position of the first value $arr[j]$ that is bigger than $arr[i]$ we know how to count the number of valid subarrays in O(1) time. But how do we figure out what $j$ is? If we do a brute force search then that becomes a O($n^2$) algorithm, but we can optimize here due to the fact that $j$ is the position of the *first value* bigger than $arr[i]$ not the absolute maximum. We can maintain a stack of indices as we iterate, each time we process a new index $i$, we can take a peek at the top of the stack. If the value at the top of the stack is greater than $arr[i]$ then we can terminate there, otherwise we can pop the head of the stack and repeat this process until $arr[stack[-1]] > arr[i]$. Then $j$ is just $stack[-1]$ and we can calculate the number of valid subarrays. Afterwards we can just add $i$ to the stack and continue processing. This way we only have a list of values used as a max previously within the stack, and we can quickly calculate $j$ without iterating over the entire array.


```
def count_subarrays(arr):
    # Process array from right -> left to see all subarrays that
    # start at i
    n = len(arr)
    r = [0 for _ in range(n)]
    stack = [n-1]
    for i in range(n-2, -1, -1):
        while stack and arr[stack[-1]] < arr[i]:
            stack.pop()
        if stack:
            r[i] = stack[-1] - i
        else:
            r[i] = n - i
        stack.append(i)
	# Process array from left -> right to see all subarrays that
	# end at i
    l = [0 for _ in range(n)]
    stack = [0]
    for i in range(1, n):
        while stack and arr[stack[-1]] < arr[i]:
            stack.pop()
        if stack:
            l[i] = i - stack[-1]
        else:
            l[i] = i + 1
        stack.append(i)

	# Consolidate results
    res = []
    for i in range(0, n):
        if l[i] == 1 and r[i] == 1:
            res.append(1)
        else:
            res.append(max(1, l[i] + r[i] - 1))
    return res
```