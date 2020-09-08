# \[Medium\] Construct Binary Tree from Preorder & Inorder Traversal

## Code

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:

    if not preorder:
        return None
    
    # first node from preorder is always the root.    
    root = TreeNode(preorder[0])
    root_index = inorder.index(preorder[0])
    
    # LeftSubtree from inorder
    #                    preorder's new left subtree, inorder's new left subtree 
    root.left = self.buildTree(preorder[1:root_index+1], inorder[:root_index])
    # RightSubtree from inorder
    root.right = self.buildTree(preorder[root_index+1:], inorder[root_index+1:])
    
    return root
    
```

