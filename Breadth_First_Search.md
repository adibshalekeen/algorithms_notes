A level-order traversal of the graph.

![[basic_tree]]

For this example tree the order would be [1, 2, 3, 4, 5, 6, 7]

```
class TreeNode:
	def __init__(self, val, left=None, right=None):
		self.val = val
		self.left = left
		self.right = right

def bfs(root: Node):
	queue = [root]
	while queue:
		node = queue.pop()
		print(node.val)
		for children in [node.left, node.right]:
			if children is not None:
				queue.append(children)
```

BFS traverses through every node in the tree so run-time is O(n)