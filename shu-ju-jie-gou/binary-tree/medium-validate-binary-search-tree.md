# \[Medium\] Validate Binary Search Tree

[Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)  
Given a binary tree, determine if it is a valid binary search tree \(BST\).  
Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.

**Example** 

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

> 思路：想想BST的特性，節點值由小到大依序為“`左 -> curr -> 右`”。  
> 因此，我們可以用BST In-order Traversal的方式來遍歷所有的節點，並且依序放到visited list裡面。  
> 如果發現到visited list裡面有不符合小至大的順序者\(ex: \[1,5,3,4,6\]\)，則 return False，全符合則 return True。   
>   
> In-Order Traversal Recursive寫法：  
> `dfs(curr, visited):  
>      dfs(curr.left, visited)  
>      visited.append(curr.val)  
>      dfs(curr.right, visited)`  
> --

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
    
    if not root:
        return True
    
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

### 2. DFS Iterative

> 思路：和DFS Recursive同樣的思維。不同的是，需要在這裡用stack來記錄需要處理的nodes。  
> 還是依照In-Order Traversal的順序"`左 -> curr -> 右`"遍歷，因此我們先把所有curr.left的所有nodes append\(\)到stack，再依序pop\(\)並且放到visited，最後再移動到curr.right。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def isValidBST(self, root: TreeNode) -> bool:

    if not root:
        return True
        
    stack = []
    visited = [] # In-order traversal. Represents the order of the visited nodes.
    curr = root
    
    while len(stack) != 0 or curr != None:
        # in-order traversal: left -> curr -> right 
        # 1.LEFT: add all left nodes
        while curr != None:
            stack.append(curr)
            curr = curr.left
            
        # 2.CURR: pop from stack and mark as visited 
        curr = stack.pop()    
        visited.append(curr.val)
        
        # 3.RIGHT: move to right node
        curr = curr.right
        
    # check validity  
    for i in range(1, len(stack)):
        if stack[i] <= stack[i-1]:
            return False
    return True
            
    
```

