# \[Easy\] Two Sum IV - Input is a BST

### Question

> ### Two Sum IV - Input is a BST   \(Easy\) 1268/131

> Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.

### Example

```text
 Test Input TreeNode as below:
 
         8
       /  \
     /      \
    3       10
   / \         \  
  1   6         14
     / \       /
    4   7     13  
    
    
 Find the target: 11
```

### Thought Process 

1. Since it is **BST**, we should use binary tree traversal.  
2. Since it is **Two Sum,** we can use **`set()`** to remember all the results that has been traversed. 
3. InOrderTraversal `(left, right, append current node)`

```python
self.inOrderTraversal(node.left, result)
self.inOrderTraversal(node.right, result)
result.append(node.val)
```

### Code

```python
class TreeNode:

	def __init__(self, val):
		self.val = val
		self.left = None
		self.right = None 

class BST_In_TwoSum:

	def findTarget(self, root, target):

		self.result = None
		self.inOrderTraverse(root, target, set())
		return self.result


	def inOrderTraverse(self, node, target, nodeSum):

		if node is None:
			return

		self.inOrderTraverse(node.left, target, nodeSum)
		self.inOrderTraverse(node.right, target, nodeSum)

		if node.val in nodeSum:
			self.result = [target - node.val, node.val]
		else:
			nodeSum.add(target - node.val)

# Driver Code 
if __name__ == "__main__": 

	root = TreeNode(8)
	root.left = TreeNode(3)
	root.left.left = TreeNode(1)
	root.left.right = TreeNode(6)
	root.left.right.left = TreeNode(4)
	root.left.right.right = TreeNode(7)

	root.right = TreeNode(10)
	root.right.right = TreeNode(14)
	root.right.right.left = TreeNode(13)

	print(BST_In_TwoSum().findTarget(root, 11))
```

Expected Result should be `[8, 3]`

