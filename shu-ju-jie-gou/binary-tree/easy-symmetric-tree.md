# \[Easy\] Symmetric Tree

## [\[Easy\] Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)        \(5760/155\)

Given the `root` of a binary tree, _check whether it is a mirror of itself_ \(i.e., symmetric around its center\).

![This is a symmetric tree](../../.gitbook/assets/image%20%2830%29.png)

```text
Ex1
[1,2,2,null,3,null,3]  --> False

Ex2
[1,2,2,2,null,2]       --> False 
```

### 1. Recursive:       O\(N\) / O\(logN\)-O\(N\)

Space Complexity: O\(N\) The worst case is that the tree is linear and the height is O\(N\) -&gt; stack size. 

{% hint style="info" %}
關鍵點：  
1\) 判斷左右是否同為None  
2\) 左樹的左分支 要等於 右樹的右分支
{% endhint %}

```python
def isSymmetric(self, root: TreeNode) -> bool:

    return self.check(root.left, root.right)

# 中心思想：
# 左樹的左分支 要等於 右樹的右分支
def check(self, left, right):
    
    # 左樹和右樹同時為None
    if not left and not right:
        return True    
    
    # 左樹或右樹只有一個為None
    if not left or not right:
        return False
    
    # 左樹的左分支 要等於 右樹的右分支
    val_eq = left.val == right.val
    symmetric_right = self.check(left.right, right.left)
    symmetric_left = self.check(left.left, right.right)

    return val_eq and symmetric_right and symmetric_left
```

### 2. Iterative: O\(N\) / O\(N\)

使用Preorder模板如下

```python
def pre_order(self, root):
    if not root:            # edge case
        return 
    stack = [root]          # 先放root到stack
    result = []
    while stack:            # while stack
        curr = stack.pop()  # pop拿出目前node
        if curr:
            result.append(curr.val)      
            if curr.right:                 # 符合條件，加到stack
                stack.append(curr.right)   
            if curr.left:                  # 符合條件，加到stack      
                stack.append(curr.left)   
    return result 
```

```python
   

def isSymmetric(self, root: TreeNode) -> bool:

    if not root:                       # edge case
        return True

    stack = [(root.left, root.right)]  # 先放root到stack

    while stack:                       # while stack
        left, right = stack.pop()      # pop出目前node
        if not left and not right:     # 易錯點：都為None，繼續
            continue

        if not left or not right:      # 左右不symmetric，False
            return False
                                       # 左樹的左分支 要等於 右樹的右分支
        if left.val == right.val:      # 符合條件，加到stack
            stack.append((left.left, right.right))
            stack.append((left.right, right.left))
        else:                          # 左右不symmetric，False
            return False
    return True
```



