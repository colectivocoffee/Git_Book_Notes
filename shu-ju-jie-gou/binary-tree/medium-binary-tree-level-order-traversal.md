# \[Medium\] Binary Tree Level Order Traversal

## Code

#### 1. BFS + Level Order\(curr\_level\): O\(logn\)

> 思路：用Queue來記錄這一層即將要掃的nodes，當掃完之後把下一層元素curr添加到curr\_level中。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def levelOrder(self, root: TreeNode) -> List[List[int]]:
    
    # init result list 
    result = []
    
    if not root:
        return result
            
    queue = deque()
    # start from root
    queue.append(root)
    
    # BFS + level order
    while len(queue) != 0:
        # since it looks like 
        # [[2,3],
        #  [4],
        #  [5,6]]
        # we need curr_level to store current level as list
        curr_level = []
        
        for _ in range(len(queue)):
            curr = queue.popleft()
            curr_level.append(curr.val) #易錯點 curr.val
            if curr.left != None:
                queue.append(curr.left)
            if curr.right != None:
                queue.append(curr.right)
        # curr_level finished, then add it to final result
        result.append(curr_level)
    return result
```

#### 2. DFS



