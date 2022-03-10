A post-order traversal of a tree

![[basic_tree]]

For the tree above we would visit each node in the following order

[4, 5, 2, 3, 6, 7, 1]

```
Class TreeNode:
	def __init__(self, val, left=None, right=None):
		self.val = val
		self.left = left
		self.right = right

def dfs(node: TreeNode):
	if node is None:
		return
	dfs(node.left)
	dfs(node.right)
	print(node.val)
```

We can also use an interative algorithm to do the same traversal in a much more painful way

```
def dfs(node: TreeNode):
	if node is None:
		return

	stack = []

	while True:
		# Populate stack in correct order (left -> right -> root)
		while node is not None:
			if node.right is not None:
				stack.append(node.right)
			stack.append(node)
			node = node.left

		node = stack.pop()
		
		if node.right is not None and stack[-1] == node.right:
			stack.pop()
			stack.append(node)
			node = node.right
		else:
			print(node.val)
			node = None

		if len(stack) == 0:
			break
		
```

To calculate the run-time, we need to take a look at the adjacency list of this tree

1 -> 2, 3
2 -> 4, 5
3 -> 6, 7
4:
5:
6:
7:

DFS for a tree traverses every node in the tree, so our runtime is O(n) where n is the number of nodes in the tree.
