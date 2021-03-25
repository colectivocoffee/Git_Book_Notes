# \[Medium\] BST Iterator

## [\[Medium\] BST Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)       \(3536/313\)

```python
class BSTIterator:
    def __init__(self, root: TreeNode):
        self.sorted_nodes = []
        self.index = -1
        self._inorder(root)
    
    def _inorder(self, curr):
        if not curr:
            return       
        self._inorder(curr.left)
        self.sorted_nodes.append(curr.val)
        self._inorder(curr.right)

    def next(self) -> int:
        self.index += 1
        return self.sorted_nodes[self.index]

    def hasNext(self) -> bool:
        
        return self.index + 1 < len(self.sorted_nodes)
```

