# \[Medium\] Flatten Binary Tree to Linked List

## [\[Medium\] Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)         \(4038/396\)

Given the `root` of a binary tree, flatten the tree into a "linked list":

* The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
* The "linked list" should be in the same order as a [**pre-order traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

![](../../.gitbook/assets/image%20%2861%29.png)

```text
Ex1
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]

Ex2
Input: root = []
Output: []
```

**Follow up:** Can you flatten the tree in-place \(with `O(1)` extra space\)?

### 1. DFS Recursive, Preorder + Flatten:      O\(N\) / O\(N\)

```python
def flatten(self, root: TreeNode) -> None:
    """
    Do not return anything, modify root in-place instead.
    """
    # step1. preorder traversal, recursive -> get result
    # step2. result -> linked list
    if not root:
        return root
        
    # step1. preorder traversal
    result = []
    def preorder(root):
        if not root:
            return
        result.append(root)
        preorder(root.left)
        preorder(root.right)
    preorder(root)

    # step2. result -> linked list
    root = result[0]
    for i in range(1,len(result)):
        root.right = result[i]
        root.left = None
        root = root.right        # linked list move a step forward
```

### 2. DFS Iterative, Preorder + Flatten:    O\(N\) / O\(N\)

```python
def flatten(self, root: TreeNode) -> None:
    """
    Do not return anything, modify root in-place instead.
    """
    if not root:
        return root

    # step1
    stack = [root]
    result = []
    while stack:
        curr = stack.pop()
        if curr:
            result.append(curr)
            if curr.right:
                stack.append(curr.right)
            if curr.left:
                stack.append(curr.left)

    #step2
    root = result[0]
    for i in range(1, len(result)):
        root.right = result[i]
        root.left = None
        root = root.right
```

### 3. O\(1\) Iterative:    O\(N\) / O\(1\)



