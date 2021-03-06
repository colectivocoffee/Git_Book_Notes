# \[Medium\] Binary Tree Traversal Series \(Inorder / Preorder / Postorder\)

## [\[Medium\] Binary Tree Inorder Traversal ](https://leetcode.com/problems/binary-tree-inorder-traversal/)    \(4356/192\)

Given the `root` of a binary tree, return _the inorder traversal of its nodes' values_.

### 1. Recursive: O\(N\) / worst O\(N\) - average O\(logN\)

Inorder Traversal 遍歷的順序是：`左->根->右`，  
因此Recursive寫法為  `root.left -> result.append -> root.right`

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        
        result = []
        return self.inorder(root, result)    
    
    def inorder(self, root, result):
    
        if not root:
            return 
        
        self.inorder(root.left, result)
        result.append(root.val)
        self.inorder(root.right, result)
        
        return result
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

### 2. Iterative: 

