# \[Easy\] Maximum Depth of a Binary Tree

## [\[Easy\] Maximum Depth of a Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)         \(3675/94\)

Given a binary tree, find its maximum depth.  
The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.  
Note: A leaf is a node with no children.

### 1. DFS Recurisve: O\(n\)/O\(n\)

Time Complexity: O\(n\) visit each node once  
Space Complexity: O\(n\) recursion would occur N times for a completely unbalanced binary tree

> 思路：用depth紀錄Recursion深度，最後在Recursion出口時，返回depth。

```python
def maxDepth(self, root: TreeNode) -> int:
    return self.dfs(0, root)
       
def dfs(self, depth, curr):
    # 遞歸出口
    if not curr:
        return depth

    return max(self.dfs(depth+1, curr.left), self.dfs(depth+1, curr.right))
    


```

### 2. BFS Iterative: O\(n\)/O\(logn\)

Time Complexity: O\(n\) visit each node **once.**  
Space Complexity: O\(logn\)  --- Height of the binary tree **O\(logn\)** 

> 思路：用典型的BFS+level order\(depth\)模板，最後return depth即可。

```python
def maxDepth(self, root: TreeNode) -> int:
    
    if not root:
        return 0
    
    depth = 0
    queue = deque()
    queue.append(root)
    
    while len(queue) != 0:
        depth += 1
        for _ in range(len(queue)):
            curr = queue.popleft()
            if curr.left != None:
                queue.append(curr.left)
            if curr.right != None:
                queue.append(curr.right)
    
    return depth
```

### 3. Iterative, DFS Preorder: O\(n\) /



```python
'''
Iterative, DFS Preorder 模板
 
    result = []
    if not root:
        return result
        
    stack = [root]
    while stack:
        curr = stack.pop()
        if curr:
            result.append(curr.val)      # 根
            if curr.left:
                stack.append(curr.left)  # 左
            if curr.right:
                stack.append(curr.right) # 右
    return result
'''
def maxDepth(self, root: TreeNode) -> int:

    depth = 0
    if not root:
        return depth

    max_depth = 1
    stack = [(root, max_depth)]
    while stack:
        curr, depth = stack.pop()
        if curr:
            max_depth = max(depth, max_depth)        # 這裡需要用max()比較max_depth
            if curr.left:
                stack.append((curr.left, depth + 1))
            if curr.right:
                stack.append((curr.right, depth + 1))

    return max_depth
```

