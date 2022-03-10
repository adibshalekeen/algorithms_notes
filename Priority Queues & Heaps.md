Priority queues are an abstract data type, and a heap is a concrete data structure used to implement a priority queue.

A priority queue is a data structure that contains a collection of items and supports the following operations:

insert: insert an item with a key
delete_min / delete_max: remove the item with the largest/smallest queue and return it

If we implement a priority queue using an array then:
	-> an unsurted array inserts would be O(1) but finding and deleting the min/max would be O(N)
	-> in a sorted array finding the min would be O(1) but inserting a value would be O(N)

Heaps are trees that support two properties:
1) The tree is almost complete (all levels are filled except the deepest level). The filled items in the last level are left-justified
2) For any node its key is greater / smaller than its parent's key (for min / max heap)

-> The key is independent of the value contained within the node
-> There is no comparable relationship between children, only the child and its parent
-> The height of a heap is guaranteed to be O(log(N))

-> Insertions
	-> Place the new key at the first leaf
	-> If property #2 is violated then perform bubble up

```
# For a min heap where the parent key must be less than the child's
def bubble_up(node):
	while node.parent exists and node.parent.key > node.key:
		swap node and node.parent
		node = node.parent
```

-> Delete min/max
	-> Delete the node with the min key and return it
	-> Reorganize the heap so properties 1) and 2) still hold
		-> Replace the root with the right-most node at the bottom of the heap and return the old root (remove it from the tree)
		-> If property #2 is violated then perform bubble down
```
# For a min heap where the parent key must be less than the child's
def bubble_down(node):
	while node is not a leaf:
		smallest_child = child of node with smallest key
		if smallest_child < node:
			swap node and smallest_child
			nade = smallest_child
```

Because the height of the tree is guaranteed to be O(log n) the run-time of both these operations is O(log n)

[k closest pointers to origin](heap/k_Closest_Pointers)
[k-th largest element in an array](heap/kth_Largest_Element_Array)
[Merge K sorted Lists](heap/Merge_k_Sorted_Lists)
[Find Median from Data Stream](heap/Find_Median_From_Stream)