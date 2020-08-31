# \[Medium\] Validate Binary Search Tree

[Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)  
Given a binary tree, determine if it is a valid binary search tree \(BST\).  
Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.

Example 

```text
    2
   / \
  1   3

Input: [2,1,3]
Output: true    
```

```text
    5
   / \
  1   4
     / \
    3   6

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

## Code

### 1. DFS Recursive \(In-order Traversal\)

Time Complexity: O\(n\)  
Space Complexity: O\(n\) Recursive call stack 

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
    
    # call recursive dfs to do in-order traversal,
    # then we will get all nodes visited
    self.dfs(root, visited)
    
    # compare values between visited[i-1] & visited[i]
    for i in range(1, len(visited)):
        if visited [i-1] >= visited[i]:
            return False
    return True
        
# in-order traversal
def dfs(self, curr, visited):
    # 遞歸出口
    if not curr:
        return 
    
    self.dfs(curr.left, visited)
    visited.append(curr.val)
    self.dfs(curr.right, visited)    
    
```

