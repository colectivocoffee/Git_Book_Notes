# \[Medium\] Path Sum II

## [\[Medium\] Path Sum II ](https://leetcode.com/problems/path-sum-ii/)                  \(2592/85\)

Given the `root` of a binary tree and an integer `targetSum`, return all **root-to-leaf** paths where each path's sum equals `targetSum`.  
A **leaf** is a node with no children.

![](../../.gitbook/assets/image%20%2832%29.png)



```text
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
```

### What is the time complexity of Binary Tree?

![single leaf tree](../../.gitbook/assets/image%20%2833%29.png)



![complete balanced tree](../../.gitbook/assets/image%20%2834%29.png)

Time Complexity: O\(N\)-O\(NlogN\)-O\(N^2\), for a balanced tree it would be O\(NlogN\), then worst case O\(N^2\)  
**Balanced Tree \(average case\)  -- O\(nlogn\)**  
Path -- each path from the root to leaf is at most of `O(logn)`, traversing from root to leaf takes O\(logn\), \(aka the height of the tree is O\(logN\)\)  
\# of leaves -- half of all nodes are leaves, and there are no more than `n/2` leaves  
**Single leaf Tree  \(best case\)  -- O\(n\)**  
Path -- Single-chain to the leaf, then `O(n)`  
\# of leaves -- `1` leaf  
**Unbalanced Tree \(worst case\)  -- O\(n^2\)**  
Path -- total \# of paths is increasing, would take up to `n/2`   
\# of leaves -- `n/2` leaves  
1 + 2 + 3 + ... n/2 = \(n/2\)\*\(\(n/2\)+1\)/2 = `n^2`

> 值得一提的是，single leaf tree 是在目前找Path Sum的情況為best case \(只有1 path\)。如果是Binary Search Tree的話，需要到leaf才能找到答案，因此這種一直線的就會是worst case。

### 1. Recursive, DFS Preorder: O\(N^2\) / O\(N\)

Time Complexity: O\(N^2\)  
Space Complexity: O\(N\)

```python
def pathSum(self, root: TreeNode, targetSum: int) -> List[List[int]]:

    result = []
    # edge case, 防止root為空，會沒辦法處理後面的root.val
    if not root:
        return result

    remainSum = targetSum - root.val
    self.preorder(root, result, remainSum, [root.val])
    return result

def preorder(self, curr, result, remainSum, path):

    if not curr:
        return

    if not curr.left and not curr.right and remainSum == 0:
        result.append(path)

    if curr.left:
        self.preorder(curr.left, result, remainSum - curr.left.val, path + [curr.left.val])
    if curr.right:
        self.preorder(curr.right, result, remainSum - curr.right.val, path + [curr.right.val])
```

### 2. Iterative, DFS Stack Preorder: O\(N^2\) / O\(N\) 

由於path sum這題要求的是`Path`本身，佐以targetSum來確定這個path是否符合條件。  
因此我們可以想到用DFS的方式往下走深挖Path，並且隨時紀錄目前總和。使用 [DFS Preorder Template](https://app.gitbook.com/@iscolectivo/s/algonote/shu-ju-jie-gou/binary-tree) 是最好選擇。  
  
那我們需要隨時維持哪些variables呢？ We should be keeping track of    
1. `current node`  
2. `remainingSum`  
3. `path so far`  
把以上的variable都存到stack裡面，同時比較看看。

```python
def pathSum(self, root: TreeNode, targetSum: int) -> List[List[int]]:

    result = []
    if not root:
        return result

    remainSum = targetSum - root.val
    # (current node, remainSum, path so far)
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

