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

![](../../.gitbook/assets/serialize-and-deserialize_binarytree.jpg)

### 1. BFS Iterative: O\(n\)/O\(n\)

> Serialization: binary tree nodes -&gt; data \(list\)  
> ssss

> **Deserialization: data -&gt; binary tree nodes**  
> 用BFS可以一層一層放node。用  
> “`curr -> curr.left (put_left == True) -> curr.right (put_left == False)`" 的順序放。

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Codec:
    """Encodes a tree to a single string.
    :type root: TreeNode
    :rtype: str
    """
    def serialize(self, root):
        if not root:
            return ""    # empty string
        
        result = []
        queue = deque()
        queue.append(root)
        
        while len(queue) != 0:
            curr = queue.popleft()
            
            # NOTE: in-order traversal
            # 中-left-right
            if curr != None:
                result.append(curr.val) # 易錯點: convert to str 
                queue.append(curr.left)
                queue.append(curr.right)
            else:
                result.appned('null')
        
        return ','.join(result)
    
    
    """Decodes your encoded data to tree.        
    :type data: str
    :rtype: TreeNode
    """
    def deserialize(self, data):
        
        if not data:
            return None
        
        # init all required nodes & BFS variables
        # req nodes:
        all_nodes = data.split(',')
        root, curr = TreeNode(int(all_nodes[0])), None
        # BFS
        queue = deque()
        queue.append(root)
        put_left = True
        
        i = 1
        while len(queue) != 0:
            # range from 1 to len(all_nodes) 
            # to make sure all data has been visited.
            # update curr only when c.left is available. 
            for _ in range(1, len(all_nodes)):
                #  Option1: LEFT
                #          curr
                #         /    \
                #  (V)c.left   c.right
                #
                if put_left == True:
                    curr = queue.popleft()
                    curr.left = self.getNode(all_nodes[i])
                    if curr.left != None:
                        queue.append(curr.left)
                # Option2: RIGHT
                #          curr
                #         /    \
                #    c.left   c.right(V)
                #                
                else:
                    curr.right = self.getNode(all_nodes[i])
                    if curr.right != None:
                        queue.append(curr.right)
                
                # all done. Increment i + 1 and flip put_left 
                put_left = not put_left
                i += 1
            
        return root
    
    # Utility method to convert string into node        
    def getNode(self, s):
        if s == 'null':
            return None
        else:
            return TreeNode(int(s))
                
```

