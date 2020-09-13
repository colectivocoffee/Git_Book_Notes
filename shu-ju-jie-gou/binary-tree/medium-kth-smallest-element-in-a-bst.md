# \[Medium\] Kth Smallest Element in a BST

[Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)  
sss

```python
def kthSmallest(self, root: TreeNode, k: int) -> int:
    # edge case
    if not root:
        return 
    # defie global variables
    self.k = k
    self.result = None
                            
    self.inorder(root)
    return self.result
    
def inorder(self, curr):
    # 遞歸出口
    if not curr:
        return
    
    # in-order traversal
    # inorder(root.left)
    # result.append(root.val)
    # inorder(root.right)
    self.inorder(curr.left)
    self.k -= 1
    if self.k == 0:
        self.result = curr.val
        return
    self.inorder(curr.right)
```







