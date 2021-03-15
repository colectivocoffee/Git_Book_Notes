# \[Medium\] Path Sum II



```python
def pathSum(self, root: TreeNode, targetSum: int) -> List[List[int]]:

    result = []
    if not root:
        return result

    
    remainSum = targetSum - root.val
    # (node, remainSum, path so far)
    stack = [(root, remainSum, [root.val])]

    while stack:
        curr, remain, path = stack.pop()
        if curr:
            if not curr.left and not curr.right and remain == 0:
                result.append(path)
            if curr.left:
                # 為什麼要用path + [curr.left.val]而不是直接append呢？
                # 我們需要一個新的path(或稱new_path_left)而非原來的path，
                # 這new_path_left需要用到new_path_left = path + [item]
                stack.append((curr.left, remain - curr.left.val, path + [curr.left.val]))
            if curr.right:
                stack.append((curr.right, remain - curr.right.val, path + [curr.right.val]))

    return result
```

