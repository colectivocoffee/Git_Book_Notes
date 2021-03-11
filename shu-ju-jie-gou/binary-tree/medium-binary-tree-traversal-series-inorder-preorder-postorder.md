# \[Medium\] Binary Tree Inorder / Preorder / Postorder Traversal Series

## [\[Medium\] Binary Tree Inorder Traversal ](https://leetcode.com/problems/binary-tree-inorder-traversal/)    \(4356/192\)

Given the `root` of a binary tree, return _the inorder traversal of its nodes' values_.

### 1. Recursive: O\(N\) / worst O\(N\) - average O\(logN\)

Inorder Traversal 遍歷的順序是：`左->根->右`，  
因此Recursive寫法為  `root.left -> result.append -> root.right`

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        
        result = []
        return self.inorder(root, result)    
    
    def inorder(self, root, result):
    
        if not root:
            return 
        
        self.inorder(root.left, result)
        result.append(root.val)
        self.inorder(root.right, result)
        
        return result
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

### 2. Iterative: O\(N\) / O\(N\)

```python
def inorderTraversal(self, root: TreeNode) -> List[int]:
    
    if not root:
        return 
    
    result = []
    stack = []
    
    while len(stack) != 0 or root != None:
        while root != None:
            stack.append(root)
            root = root.left            
        root = stack.pop()
        result.append(root.val)
        root = root.right
        
    return result
```

### 3. Morris Traversal: O\(N\) / O\(N\)

## [\[Medium\] Binary Preorder Traversal ](https://leetcode.com/problems/binary-tree-preorder-traversal/)          \(2097/86\)

Given the `root` of a binary tree, return _the preorder traversal of its nodes' values_.

### 1. Recursive: O\(N\) / worst O\(N\) - avg O\(logN\)

Preorder 遍歷的順序是 `根->左->右`  
因此Recursive寫法為  `result.append -> root.left -> root.right`

```python
def preorderTraversal(self, root: TreeNode) -> List[int]:
    result = []
    return self.preorder(root, result)

def preorder(self, root, result):
    if not root:
        return 
    
    result.append(root.val)
    self.preorder(root.left, result)
    self.preorder(root.right, result)
    
    return result
```

### 2. Iterative: O\(N\) / O\(N\)

```python
def preorderTraversal(self, root: TreeNode) -> List[int]:

    if not root:
        return 

    result = []
    stack = []

    # root->left->right
    while len(stack) != 0 or root != None:
        while root != None:
            result.append(root.val)
            stack.append(root)
            root = root.left

        curr = stack.pop()
        root = curr.right

    return result
```

### 3. Morris Traversal: O\(N\) / O\(N\)

## \[Medium\] Binary Tree Postorder Traversal 

Given the `root` of a binary tree, return _the postorder traversal of its nodes' values_.

### 1. Recursive: O\(N\) / O\(N\)

### 2. Iterative: O\(N\) / O\(N\)

postorder traversal: `左 -> 右 -> 根`

```python
def postorderTraversal(self, root: TreeNode) -> List[int]:

    if not root:
        return

    stack = [root]
    result = collections.deque()

    while stack:
        curr = stack.pop()
        if curr:
            if curr.left:
                stack.append(curr.left)
            if curr.right:
                stack.append(curr.right)
            result.appendleft(curr.val)     #易錯點：deque.appendleft

    return result
```

## [\[Easy\] N-Tree Postorder Traversal](https://leetcode.com/problems/n-ary-tree-postorder-traversal/)          \(926/73\)

### 1. Recursive: O\(N\) / O\(N\) 

Time Complexity: O\(N\) where N is the number of Nodes  
Space Complexity: O\(N\) up to the entire tree

```python
def postorder(self, root: 'Node') -> List[int]:
    result = []
    return self.post(root, result)

def post(self, root, result):

    if not root:
        return
    # 跟Binary Tree唯一不同的是，用一個forloop取代原來的root.left & root.right traversal。 
    for child in root.children:
        self.post(child, result)
    result.append(root.val)

    return result
```

### 2. Iterative: O\(N\) / O\(N\)

```python
def postorder(self, root: 'Node') -> List[int]:

    if not root:
        return 

    stack = [root]
    result = collections.deque()

    while stack:
        curr = stack.pop()
        if curr:
            for child in curr.children:
                stack.append(child)
            result.appendleft(curr.val)

    return result
```

#### Reference

* Preorder: [https://leetcode.wang/leetcode-144-Binary-Tree-Preorder-Traversal.html](https://leetcode.wang/leetcode-144-Binary-Tree-Preorder-Traversal.html)
* Inorder: [https://leetcode.wang/leetCode-94-Binary-Tree-Inorder-Traversal.html](https://leetcode.wang/leetCode-94-Binary-Tree-Inorder-Traversal.html)
* Postorder: [https://leetcode.wang/leetcode-145-Binary-Tree-Postorder-Traversal.html?h=postorder](https://leetcode.wang/leetcode-145-Binary-Tree-Postorder-Traversal.html?h=postorder) 

