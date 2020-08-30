# \[Medium\] Validate Binary Search Tree

Validate Binary Search Tree  


## Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def isValidBST(self, root: TreeNode) -> bool:
    
    # to keep a list of visited nodes to compare values later 
    visited = []
    
    # call recursive dfs to do in-order traversal
    self.dfs(root, visited)
    
    # compare values between visited[i-1] & visited[i]
    for i in range(1, len(visited)):
        if visited [i-1] >= visited[i]:
            return False
    
    return True
        
# in-order traversal
def dfs(self, curr, visited):

    if not curr:
        return 
    
    self.dfs(curr.left, visited)
    visited.append(curr.val)
    self.dfs(curr.right, visited)    
    
```

