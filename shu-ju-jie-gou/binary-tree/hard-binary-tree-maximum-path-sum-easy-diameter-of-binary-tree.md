# \[Hard\] Binary Tree Maximum Path Sum \[Easy\] Diameter of Binary Tree

Maximum Path Sum 和 Diameter of Binary Tree 其實都是一樣的思維。  
Maximum Path Sum 找的是**Binary Tree同一條path上所有節點的值的最大總和**，而  
Diameter 找的是**Binary Tree上包含最多節點的path**。

#### Traverse時注意要點：

![](../../.gitbook/assets/maxpathsum.jpg)

#### Iterative Postorder注意要點：

> 1. 找添加子樹時的最大值 = `max( (1)選左子樹(2)選右子樹(3)都不選0 )` 
> 2. 用`dictionary`紀錄目前節點最大值
> 3. `Traverse兩次`。第一次Postorder traversal看完所有節點並加到traverse\_list，第二次用for loop比較每個子樹加or不加，看哪個最大。

當遇到需要看所有節點的題型時，我們可以很自然地想到要用Binary Tree xxorder來解。然而在traverse時，會需要走回頭路才能遍歷所有的nodes。我們不能在第一次遍歷的時候，就把所有答案加上去，這樣會造成走回頭路的值也被加到答案裡。  
我們需要遍歷兩次，一次是用postorder traversal看樹的長相，第二次才用for loop做比大小。

## [\[Hard\] Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)          \(5306/384\)

Given a **non-empty** binary tree, find the maximum path sum.  
For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections.   
The path must contain **at least one node** and does not need to go through the root.

```text
Example
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
```

### 1. Recursive, DFS:   O\(N\) / O\(H\)

* Time complexity: `O(N)`, where `N` is number of nodes, since we visit each node not more than 2 times.
* Space complexity: `O(H)`, where `H` is a tree height, to keep the recursion stack. In the average case of the balanced tree, the tree height `H = logN`, in the worst case of skewed tree, `Height = N`.

> 思路：同時用recursion left加總左子樹的值，recursion right加總右子樹的值，再比較\(1\)`左子樹+root/curr.val` , \(2\)`右子樹+root/curr.val`, \(3\)`0` 哪個比較大。  
> 同場加映，為什麼不能直接用iterative preorder來處理呢？  
> 雖然是可以，但需要額外處理unbalanced tree的各種情況。  
> 當子樹下有多層children nodes\(一串葡萄\) vs 單一leaf node\(一顆葡萄\) 時，不能直接把leaf node值加入，需要先比較children nodes的總和是否有比leaf node大，等比較完後才能加入。  
> 可以選擇用iterative postorder解題。

```python
可以測試的edge case
root = 5
[5,4,8,11,null,13,4,7,2,null,null,null,1]
result = 48  #(not 55)

# traverse每一個node再回來，就會得到55  (X)
# 一串葡萄vs一顆葡萄只選一個，就會得到48  (O)
```

![](../../.gitbook/assets/max_path_sum.jpg)

```python
def maxPathSum(self, root: TreeNode) -> int:
    if not root:
        return 0
        
    # global variable to track max path sum
    # 需要用 -infinity 來開頭
    self.maxSum = float('-inf')
    # start the recursion from the root
    self.countPath(root)
    return self.maxSum
    
#        3
#      /   \
#     1     8
#    / \   / \
#   6   7 9  10
# n n  n n ....
def countPath(self, curr):
    # 遞歸出口
    # 如果leaf node(n)沒有東西了，代表curr node下面的sum為0
    if not curr:
        return 0
        
    leftSum = self.countPath(curr.left)
    rightSum = self.countPath(curr.right)
    # 如果找到max path sum則記錄下來，反之則維持maxSum的原樣
    self.maxSum = max(self.maxSum, leftSum + rightSum + curr.val)
    # 最重要的一步：max path sum只能從三種情況選一種
    # (1) leftSum + root
    # (2) rightSum + root
    # (3) 0
    return max(leftSum + curr.val, rightSum + curr.val, 0)
    
```

### 2. Iterative, DFS Postorder: O\(N\) / O\(H\)

```python
def maxPathSum(self, root: TreeNode) -> int:

    max_sum = float('-inf')
    if not root:
        return max_sum
        
    # step1.第一次traverse：看樹長相
    # Postorder gives us Left->Right->Root which matches with our requirement
    # 我們需要比較看看(1)左子樹sum (2)右子樹sum (3)都不選0 哪個大，
    # 因此要先看左右，再看root -> postorder
    traverse_list = self.postorder(root)
    
    # step2.用dictionary紀錄最大值
    # 用dictionary紀錄到[node]為止的path_sum
    path_sum = {None: 0}
    # step3.第二次traverse：比大小
    # 第二次過所有的nodes，這次把left_max, right_max, max_sum都算出來，並且記錄到path_sum上。
    for node in traverse_list:
        left_max = max(path_sum[node.left],0)
        right_max = max(path_sum[node.right],0)
        path_sum[node] = max(left_max, right_max) + node.val
        max_sum = max(max_sum, left_max + right_max + node.val)       
    return max_sum


def postorder(self, root):
    if not root:
        return
    traverse_list = collections.deque()
    stack = [root]
    while stack:
        curr = stack.pop()
        if curr:
            if curr.left:
                stack.append(curr.left)
            if curr.right:
                stack.append(curr.right)
            traverse_list.appendleft(curr)
    return traverse_list
```

## [\[Easy\] Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)      \(4399/277\)

### 1. Recursive, DFS:      O\(N\) / O\(N\)

```python
def diameterOfBinaryTree(self, root: TreeNode) -> int:
    if not root:
        return 0
        
    self.diameter = 0
    self.countDiameter(root)
    return self.diameter
    
def countDiameter(self, curr):
    if not curr:
        reutn 0
    
    leftSum = self.countDiameter(curr.left)
    rightSum = self.countDiameter(curr.right)
    
    self.diameter = max(self.diameter, leftSum + rightSum)
    
    return max(leftSum + 1, rightSum + 1)
    
```

### 2. Iterative, DFS Postorder:   O\(N\) / O\(H\)

```python
def diameterOfBinaryTree(self, root: TreeNode) -> int:
    count = 0
    if not root:
        return count
    # step1.第一次traverse：看樹長相
    traverse_list = self.postorder(root)
    # step2.用dictionary紀錄最大值
    diameter_dict = {None : 0}
    # step3.第二次traverse：比大小
    for node in traverse_list:
        left_max = max(diameter_dict[node.left], 0)
        right_max = max(diameter_dict[node.right], 0)            
        diameter_dict[node] = max(left_max, right_max) + 1
        count = max(count, left_max + right_max)
    return count

def postorder(self, root):
    if not root:
        return
    traverse_list = collections.deque()
    stack = [root]
    while stack:
        curr = stack.pop()
        if curr:
            if curr.left:
                stack.append(curr.left)
            if curr.right:
                stack.append(curr.right)
            traverse_list.appendleft(curr)
    return traverse_list
```

#### Reference

* Maximum Path Sum Iterative Postorder Solution [https://leetcode.com/problems/binary-tree-maximum-path-sum/discuss/232373/Postorder-Iterative-Solution-Python](https://leetcode.com/problems/binary-tree-maximum-path-sum/discuss/232373/Postorder-Iterative-Solution-Python)

