# \[Hard\] Serialize and Deserialize a Binary Tree

[Serialize and Deserialize a Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)  
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Example**

```text
You may serialize the following tree:

    1
   / \
  2   3
     / \
    4   5

as "[1,2,3,null,null,4,5]"
```

## Code

### 1. BFS Iterative: O\(n\)/O\(n\)

{% tabs %}
{% tab title="Python" %}
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Codec:

def serialize(self, root):
"""Encodes a tree to a single string.
:type root: TreeNode
:rtype: str
"""
        if not root:
                return
                 
        result = []
        queue = deque()
        queue.append(root)
        
        while len(queue) != 0:
                curr = queue.popleft()
                # curr is not null
                if curr != None:
                        result.append(curr.val)
                                                



def deserialize(self, data):
"""Decodes your encoded data to tree.        
:type data: str
:rtype: TreeNode
"""
```

>
{% endtab %}
{% endtabs %}

