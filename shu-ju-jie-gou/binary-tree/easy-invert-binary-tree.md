# \[Easy\] Invert Binary Tree

[Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

**Example:**

```text
From
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

```text
To
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

## Code

### 1. DFS Recursive: O\(n\)/O\(n\)

Time Complexity: O\(n\) n is the number of nodes, every node has to be traversed once  
Space Complexity: O\(n\) it is actually O\(h\) h denotes the height of the tree, but here it is close to O\(n\)

> 思路：在dfs遞歸的時候，把curr.left & curr.right 互換，並且加上一個temp來記錄即可。

```python
def invertTree(self, root: TreeNode) -> TreeNode:
    
    self.dfs(root)
    return root
    
    
# swap the left&right node while doing dfs    
def dfs(self, curr):
    
    if not curr:
        return 
    
    self.dfs(curr.left)
    self.dfs(curr.right)
    
    temp = curr.left
    curr.left = curr.right
    curr.right = temp


```

### 2. BFS Iterative: O\(n\)/O\(n/2\)

```python
def invertTree(self, root: TreeNode) -> TreeNode:
    if not root:
        return None
        
    queue = deque()
    queue.append(root)
    
    while len(queue) != 0:
        curr = queue.popleft()
        
        # swap left & right
        temp = curr.left
        curr.left = curr.right
        curr.right = temp
        
        if curr.left != None:
            queue.append(curr.left)
        if curr.right != None:
            queue.append(curr.right)
    
    return root
```

