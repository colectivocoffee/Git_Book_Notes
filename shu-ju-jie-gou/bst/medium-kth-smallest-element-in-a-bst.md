# \[Medium\] Kth Smallest Element in a BST

## [\[Medium\] Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)             \(3644/83\)

Given a binary search tree, write a function `kthSmallest` to find the **k**th smallest element in it.

```text
Example
Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

## Code

![](../../.gitbook/assets/image%20%2844%29.png)

### 1. In-order Traversal Recursive:      O\(N\) / O\(N\)

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

### 2. In-order Recursive + sorted array特性:   O\(H+k\) / O\(H\)

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

### 2. In-order Traversal Iterative:   O\(H+k\) / O\(H\)

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

```python
def kthSmallest(self, root: TreeNode, k: int) -> int:
    # init
    i = 0
    stack = []
    
    while root or stack:
        while root:
            stack.append(root)
            root = root.left
        # 注意，這裡最好不要用curr = stack.pop()
        # 在 [1,null,2], k=2的情況下會失敗，沒辦法回來
        root = stack.pop()
        i += 1
        if k == i:
            return root.val
        root = root.right
```

\*\*\*\*

**Follow up:**  
What if the BST is modified \(insert/delete operations\) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

\[Ans\] We could add a variable to the TreeNode to record the size of the left subtree. When insert or delete a node in the left subtree, we increase or decrease it by 1. So we could know whether the kth smallest element is in the left subtree or in the right subtree by compare the size with k.

----

\[Official Ans\] Without any optimization insert/delete + search of kth element has O\(2H+k\) complexity. How to optimise that?

That's a design question, basically, we're asked to implement a structure which contains a BST inside and optimizes the following operations :

* Insert
* Delete
* Find kth smallest

Seems like a database description, isn't it? Let's use here the same logic as for [**LRU cache**](https://leetcode.com/articles/lru-cache/) design, and combine an indexing structure \(we could keep BST here\) **with a double-linked list**.

Such a structure would provide:

* O\(H\) time for the insert and delete.
* O\(k\) for the search of kth smallest.

![](../../.gitbook/assets/image%20%2853%29.png)

