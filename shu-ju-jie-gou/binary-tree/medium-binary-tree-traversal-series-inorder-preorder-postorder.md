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

### 2. Iterative: O\(N\) / O\(N\)

```python
def inorderTraversal(self, root: TreeNode) -> List[int]:
    
    if not root:
        return 
    
    result = []
    stack = []
    
    while len(stack) != 0 or root != None:
        while root != None:
            stack.append(root)
            root = root.left            
        root = stack.pop()
        result.append(root.val)
        root = root.right
        
    return result
```

### 3. Morris Traversal: O\(N\) / O\(N\)

## \[Medium\] Binary Preorder Traversal           \(2097/86\)

Given the `root` of a binary tree, return _the preorder traversal of its nodes' values_.

### 1. Recursive: O\(N\) / worst O\(N\) - avg O\(logN\)

```python
def preorderTraversal(self, root: TreeNode) -> List[int]:
    result = []
    return self.preorder(root, result)

def preorder(self, root, result):
    if not root:
        return 
    
    result.append(root.val)
    self.preorder(root.left, result)
    self.preorder(root.right, result)
    
    return result
```

### 2. Iterative: O\(N\) / O\(N\)

```python
def preorderTraversal(self, root: TreeNode) -> List[int]:

    if not root:
        return 

    result = []
    stack = []

    # root->left->right
    while len(stack) != 0 or root != None:
        while root != None:
            result.append(root.val)
            stack.append(root)
            root = root.left

        curr = stack.pop()
        root = curr.right

    return result
```

