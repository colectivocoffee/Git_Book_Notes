# \[Medium\] Binary Tree Zigzag Level Order Traversal

## [\[Medium\] Binary Tree Zigzag Level Order Traversal ](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)          \(3218/726\)

Given a binary tree, return the zigzag level order traversal of its nodes' values. \(ie, from left to right, then right to left for the next level and alternate between\).

For example:  
Given binary tree `[3,9,20,null,null,15,7]`

```text
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:

```text
[
  [3],
  [20,9],
  [15,7]
]
```

## Code

### 1. Binary Tree BFS:

> 思路：看到題目要求是Level Order Traversal，我們就可以用Binary Tree BFS分層遍歷的特性來解題。  
> 唯一需要注意的是題目要求zigzag，我們需要reverse level % 2 == 0的值。

```python
def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
    if not root:
        return []
        
    result = []
    queue = deque()
    queue.append(root)
    
    level = 0
    while len(queue) != 0:
        curr_level = []
        level += 1
        for _ in range(len(queue)):
            curr = queue.popleft()
            curr_level.append(curr)
            # append all the childrens to the queue
            if curr.left != None:
                queue.append(curr.left)
            if curr.right != None:
                queue.append(curr.right)
        # 需要zigzag，因此用 % 來取remainder看level
        if level % 2 == 0:
            result.append(curr_level[::-1])
        else:
            result.append(curr_level)
    return result        
```

### 2. Iterative, BFS Level Order: 

```python
def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:

    result = []
    if not root:
        return result

    stack = [(root,0)]

    while stack:
        curr, level = stack.pop()
        if level == len(result):
            new_level = collections.deque()
            result.append(new_level)

        if level % 2 == 0:
            result[level].append(curr.val)
        elif level % 2 != 0:
            result[level].appendleft(curr.val)

        if curr.right:
            stack.append((curr.right, level + 1))
        if curr.left:
            stack.append((curr.left, level + 1))

    return result
```

