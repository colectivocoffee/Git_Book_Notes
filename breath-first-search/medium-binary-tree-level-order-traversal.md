# \[Medium\] Binary Tree Level Order Traversal

## [\[Medium\] Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)    \(4343 / 108\)

Given a binary tree, return the level order traversal of its nodes' values. \(ie, from left to right, level by level\).

```text
Example
    3             [ 
   / \              [3],
  9  20     ==>    [9,20],
    /  \           [15,7]
   15   7         ]
```

## Code

### 1. BFS Iterative + Level Order\(curr\_level\): O\(logn\) / \(Recommend\)

> 思路：Top-Down Approach  
> 用Queue來記錄這一層除了curr之外，即將要掃的所有left&right nodes都放到裡面。當掃到curr node時，把curr添加到curr\_level中。

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

### 2. DFS Recursive + PreOrder Traversal: 

> 思路：Top-Down Approach，一層一層往下掃。  
> 由於是Binary Tree，  
> dfs\(level+1, curr.left\)  
> dfs\(level+1, curr.right\)  
> 會先把同一層的左右nodes都掃完後，才會繼續往下一層走。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def levelOrder(self, root: TreeNode) -> List[List[int]]:
    result = []
    self.dfs()
    return result
    
def dfs(self, level, curr, result):
    # 遞歸出口，即後面沒有node了
    if not curr:
        return 
    # 到了下一層發現還沒有curr_level，新增一個 []
    if level == len(result):
        result.append([])
    
    #    3
    #   / \
    #  9  20
    #    /  \
    #   15   7
    #[[]]
    #[[3]] 3
    #[[3], []]
    #[[3], [9]] 9
    #[[3], [9, 20]] 20
    #[[3], [9, 20], []]
    #[[3], [9, 20], [15]] 15
    #[[3], [9, 20], [15, 7]] 7
    # Add curr to curr_level
    result[level].append(curr.val)
    self.dfs(level+1, curr.left, result)
    self.dfs(level+1, curr.right, result)    
    
```

### 3. DFS Iterative:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def levelOrder(self, root: TreeNode) -> List[List[int]]:
    
    result = []
    if not root:
        return result
    
    stack = []
    stack.append((root,0))
    while len(stack) != 0:
        curr, level = stack.pop()  # pop the last item on right
        if level == len(result):
            result.append([])
        
        result[level].append(curr.val)
        # 注意，這裡是先right後left
        # 因為 stack.pop()是LIFO 
        # 這樣才能    In: right->left 
        #           Out:        left -> right 
        if curr.right != None:
            stack.append((curr.right, level+1))
        if curr.left != None:
            stack.append((curr.left, level+1))
    
    return result
```

#### 3. Bottom-Up Approach DFS

#### 4. Bottom-Up Approach BFS



