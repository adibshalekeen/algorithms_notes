-> A post-order traversal of a tree

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

To calculate the run-time, we need to take a look at the adjacency list of this tree

1 -> 2, 3
2 -> 4, 5
3 -> 6, 7
4:
5:
6:
7:

At each node, we will have to traverse each node 