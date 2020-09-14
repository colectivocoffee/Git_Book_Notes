# \[Medium\] Kth Smallest Element in a BST

[Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)  
sss

### 1. In-order Traversal Recursive

```python
def kthSmallest(self, root: TreeNode, k: int) -> int:
    # edge case
    if not root:
        return 
    # defie global variables
    self.k = k
    self.result = None
                            
    self.inorder(root)
    return self.result
    
def inorder(self, curr):
    # 遞歸出口
    if not curr:
        return
    
    # in-order traversal
    # inorder(root.left)
    # result.append(root.val)
    # inorder(root.right)
    self.inorder(curr.left)
    self.k -= 1
    if self.k == 0:
        self.result = curr.val
        return
    self.inorder(curr.right)
```

### 2. In-order Recursive + sorted array特性

> 思路：\(1\)用In-order Traversal的方式，因為**BST Inorder有序**。  
> \(2\) 又因Inorder有序，result list會以ascending order的方式排列，我們可以利用它取`result[k-1]` 就可以得到Kth Smallest。

使用result list來記錄看每一個到i th smallest的node，等traversal結束後，用`result[k-1]`的方式取出Kth Smallest Node。

```python
def kthSmallest(self, root: TreeNode, k: int) -> int:
    if not root:
        return
    result = []
    
    def inorder(curr):
        if not curr:
            return
        # inorder traversal
        inorder(curr.left)
        result.append(curr.val)
        if len(result) == k:
            return
        inorder(curr.right)
    
    inorder(root)
    return result[k-1]
```

### 2. In-order Traversal Iterative

```python
def kthSmallest(self, root: TreeNode, k: int) -> int:
    if not root:
        return
    
    result = []
    stack = []
                            
    while root != None or len(stack) != 0:
        # inorder traversal 3 steps
        # 1.check all the LEFT nodes and append them to stack 
        while root != None:
            stack.append(root)
            root = root.left
        # 2.POP right from stack
        #   add root to result list
        root = stack.pop()
        result.append(root.val) 
        # 3.check RIGHT node
        root = root.right
        
    return result[k-1]
        


```







