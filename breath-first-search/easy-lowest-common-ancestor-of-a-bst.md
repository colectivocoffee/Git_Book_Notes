# \[Easy\] Lowest Common Ancestor of a BST

[Lowest Common Ancestor of a BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)  
2247/110  
Given a binary search tree \(BST\), find the lowest common ancestor \(LCA\) of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants \(where we allow **a node to be a descendant of itself**\).”



## Code

### 1. DFS Recursion \(same as LCA Binary Tree\)

```python
和Lowest Common Ancestor of a Binary Tree 相同寫法
```

### 2. DFS Recursion \(利用BST特性\)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':

    return self.dfs(root, p, q)

def dfs(self, root, p, q):
    
    pVal = p.val
    qVal = q.val
    
    if not root:
        return root
        
    # BST特性：                                  中(root)
    # 左子樹的所有值一定都小於root.val，          /           \
    # 右子樹的所有值一定都大於root.val           小           大 
    #                                      /   \        /   \
    #                                    最小  小中    中大   最大
    if pVal < root.val and qVal < root.val:
        return self.dfs(root.left, p, q)
    elif pVal > root.val and qVal > root.val:
        return self.dfs(root.right, p, q)
    else:
        return root    
```

