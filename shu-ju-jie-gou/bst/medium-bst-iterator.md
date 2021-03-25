# \[Medium\] BST Iterator

## [\[Medium\] BST Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)       \(3536/313\)

### 1. DFS Recursive, Inorder:   O\(N\) / O\(N\)

Time Complexity: `O(N)` is the time taken by the constructor for the iterator. The problem statement only asks us to analyze the complexity of the two functions, however, when implementing a class, it's important to also note the time it takes to initialize a new object of the class and in this case, it would be linear in terms of the number of nodes in the BST. In addition to the space occupied by the new array we initialized, the recursion stack for the inorder traversal also occupies space but that is limited to O\(h\) where h is the height of the tree.

* `next()` would take O\(1\)
* `hasNext()` would take O\(1\)

Space Complexity: `O(N)` since we create a new array to contain all the nodes of the BST. This doesn't comply with the requirement specified in the problem statement that the maximum space complexity of either of the functions should be O\(h\) where h is the height of the tree and for a well-balanced BST, the height is usually logN. So, we get great time complexities but we had to compromise on the space. Note that the new array is used for both the function calls and hence the space complexity for both the calls is O\(N\).

```python
class BSTIterator:
    # init would take linear O(n) time
    # init takes O(n) space for new array sorted_nodes
    def __init__(self, root: TreeNode):
        self.sorted_nodes = []
        self.index = -1
        self._inorder(root)
    
    # Internal Method to do recursive inorder BST traversal
    def _inorder(self, curr):
        if not curr:
            return       
        self._inorder(curr.left)
        self.sorted_nodes.append(curr.val)
        self._inorder(curr.right)
    
    # next would take O(1) time
    def next(self) -> int:
        self.index += 1
        return self.sorted_nodes[self.index]
    
    # hasNext would take O(1) time
    def hasNext(self) -> bool:
        return self.index + 1 < len(self.sorted_nodes)
```

### 2. DFS Iterative, Inorder:   O\(N\) / O\(N\)

```python
'''
Inorder模板

def inorder(root):
    if not root:
        return 
    result = []
    stack = []

    while root or stack:
        while root:
            stack.append(root)  # 左左左...
            root = root.left    
        root = stack.pop()      # 節點為空，就出棧
        result.append(root.val) # inorder result
        root = root.right       # 看右子樹
    return result
'''
class BSTIterator:

    def __init__(self, root: TreeNode):
        self.stack = []
        self._inorder(root)
    
    def _inorder(self, root):
        if not root:
            return
        
        while root:
            self.stack.append(root)
            root = root.left
            
    def next(self) -> int:
        
        curr = self.stack.pop()
        if curr.right:
            self._inorder(curr.right)
        return curr.val
        

    def hasNext(self) -> bool:
        return len(self.stack) > 0
```

