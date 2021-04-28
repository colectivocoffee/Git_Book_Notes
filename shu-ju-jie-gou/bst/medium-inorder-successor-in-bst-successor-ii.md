# \[Medium\] Inorder Successor in BST / Successor II

## \[Medium\] Inorder Successor in BST

還沒解

## [\[Medium\] Inorder Successor in BST II](https://leetcode.com/problems/inorder-successor-in-bst-ii/)       \(457/25\) locked

Given a `node` in a binary search tree, return _the in-order successor of that node in the BST_. If that node has no in-order successor, return `null`.  
The successor of a `node` is the node with the smallest key greater than `node.val`.  
You will have direct access to the node but not to the root of the tree. Each node will have a reference to its parent node. Below is the definition for `Node`:

```text
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
}

```

![](../../.gitbook/assets/image%20%2870%29.png)

```text
Input: tree = [15,6,18,3,7,17,20,2,4,null,13,null,null,null,null,null,null,null,null,9], node = 15
Output: 17
```

#### Successor and Predecessor

> Successor = "after node", i.e. the next node in the inorder traversal, or the smallest node _after_ the current one.

> Predecessor = "before node", i.e. the previous node in the inorder traversal, or the largest node _before_ the current one.

![](../../.gitbook/assets/image%20%2858%29.png)

### 1. Build BST + Convert to List + Find i+1:  O\(N\) / O\(N\)

```python
def inorderSuccessor(self, node: 'Node') -> 'Node':
    
    if not node:
        return node
    
    # step1. find root of the tree
    root = node
    while root.parent:
        root = root.parent
        
    # step2. inorder traversal
    result = []
    self.inorder(root, result)
    
    # step3. find node in the result
    for i in range(len(result)-1):
        if result[i] == node.val:
            return TreeNode(result[i+1])
    return None

def inorder(self, root, result):
    if not root:
        return
    self.inorder(root.left, result)
    result.append(root.val)
    self.inorder(root.right, result)
```

### 2. BST Traverse, by checking right/left/parent:    O\(N\) / O\(1\)

1. If the node has a right child, and hence its successor is somewhere lower in the tree. Go to the right once and then as many times to the left as you could. Return the node you end up with.
2. Node has no right child, and hence its successor is somewhere upper in the tree. Go up till the node that is _left_ child of its parent. The answer is the parent.

![Case1: Successor&#x5728;node&#x4E0B;&#x65B9;](../../.gitbook/assets/image%20%2874%29.png)

![Case2: successor&#x5728;node&#x4E0A;&#x65B9;](../../.gitbook/assets/image%20%2861%29.png)

```python
def inorderSuccessor(self, node: 'Node') -> 'Node':
    
    # Case1: successor在node下方
    # 先往右一格，再左左左左
    if node.right:
        node = node.right
        while node.left:
            node = node.left
        return node
    
    # Case2: successor在node上方
    # go up and find the node with greater value
    # 一直往上走，直到node < node.parent時，node.parent就是答案
    while node.parent and node.val > node.parent.val:
        node = node.parent
    return node.parent
```

