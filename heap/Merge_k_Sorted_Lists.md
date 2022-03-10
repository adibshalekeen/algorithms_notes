Given k sorted lists of numbers, merge them into one sorted list

ex.

Input: $[[1, 3, 5], [2, 4, 6], [7, 10]]$

Output: $[1, 2, 3, 4, 5, 6 , 7, 10]$

Solution:

We generate a stream of values from all of the input arrays. We can insert the values into a heap, and as we pop them out we can increment the pointers for each list and insert the new values.

```
import heapq
def mergeKSortedLists(def lists: List[List[int]]) -> List[int]:
	h = []
	for i, list in enumerate(lists):
		if list:
			heapq.heappush(h, (list[0], (i, 0)))
	result = []
	while h:
		node = heapq.heappop(h)
		result.append(node[0])
		list_index = node[1][0]
		new_val_index = node[1][1] + 1
		if new_val_index < len(lists[list_index]):
			heapq.heappush(h, (lists[list_index][new_val_index], (list_index, new_val_index)))
	return result
	
```

The run time of this algorithm is $O(n log n)$ 

We can do something similar if the input is stored as a list of Linked Lists.

```
import heapq
def mergeKLists(lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        h = []
        for i, l in enumerate(lists):
            if l is not None:
                heapq.heappush(h, (l.val, i, l))
                count += 1
        sentinal_head = ListNode(-1)
        head = sentinal_head
        while h:
            node = heapq.heappop(h)
            new_node = ListNode(node[0])
            next_node = node[2].next
            head.next = node[2]
            head = head.next
            head.next = None
            if next_node is not None:
                heapq.heappush(h, (next_node.val, node[1], next_node))
        return sentinal_head.next
```
