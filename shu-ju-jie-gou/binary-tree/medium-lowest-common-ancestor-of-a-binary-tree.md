# \[Medium\] Lowest Common Ancestor of a Binary Tree

## [\[Medium\] Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) \(4056/178\)

Given a binary tree, find the lowest common ancestor \(LCA\) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants \(where we allow **a node to be a descendant of itself**\).”

**Example**

![](../../.gitbook/assets/image.png)

```text
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

```text
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, 
since a node can be a descendant of itself according to the LCA definition.
```

## **Code**

![](../../.gitbook/assets/lowestcommonancestor.jpg)

### 1. DFS Recursive: O\(N\) / O\(N\)

Time Complexity: O\(n\)  
Space Complexity: O\(n\)

> 思路：根據題意，要我們找“ find the lowest common ancestor \(LCA\) of two given nodes in the tree ”  
> 對於given nodes p＆q，只有兩種情況：\(1\)找到\(2\)沒找到。要嘛找到return left or right，要嘛沒找到return None。  
> 又LCA只給我們以下三種可能情況：  
> \(1\) p,q 分別在`左右子樹`   
> \(2\) p,q 都在`右子樹`   
> \(3\) p,q 都在`左子樹`  
> 所以，用DFS Recursive來找是最容易的方法。  
> 當我們DFS Recursion完後，左右分別都回到root節點3，在Ex2情況下，root左邊返回5，root右邊返回None，而p,q自己就是LCA。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':

    return self.dfs(root, p, q)
    
def dfs(self, root, p, q):
    
    # 遞歸出口
    if not root or root == p or root == q:
        return root
        
    # recursive call在condition check之後 --> top-down  
    left = self.dfs(root.left, p, q)
    right = self.dfs(root.right, p, q)
    
    # p,q 分別在左右子樹 => root本身為最小LCA
    if left != None and right != None:
        return root
        
    # 如果p,q不在左子樹 => p,q都在右子樹 => root.right為最小LCA 
    if not left:
        return right
        
    # 如果p,q不在右子樹 => p,q都在左子樹 => root.left為最小LCA
    if not right:
        return left

```

### 2. DFS Iterative + Parent Pair:     O\(N\) / O\(N\)

Intuition:   
If we have parent pointers for each node we can traverse back from `p` and `q` to get their ancestors. The first common node we get during this traversal would be the LCA node. We can save the parent pointers in a dictionary as we traverse the tree.

1. Start from the root node and traverse the tree.
2. Until we find `p` and `q` both, keep storing the parent pointers in a dictionary.
3. Once we have found both `p` and `q`, we get all the ancestors for `p` using the parent dictionary and add to a set called `ancestors`.
4. Similarly, we traverse through ancestors for node `q`. If the ancestor is present in the ancestors set for `p`, this means this is the first ancestor common between `p` and `q` \(while traversing upwards\) and hence this is the LCA node.

```python
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':

    stack = [root]

    # parent-child pair
    all_parents = {root: None}
    
    # 把所有parent-child關係都存起來，直到找到 p&q
    while p not in all_parents or q not in all_parents:

        curr = stack.pop()

        if curr.left:
            all_parents[curr.left] = curr
            stack.append(curr.left)
        if curr.right:
            all_parents[curr.right] = curr
            stack.append(curr.right)
    
    # ancestors of node p
    ancestors = set()
    
    # 用parent-child pair
    # 找p的所有ancestors祖宗，並且加到ancestors set
    while p:
        ancestors.add(p)
        p = all_parents[p]
    
    # 利用q，大部分都找不到，直到找第一個出現在ancestors就返回
    while q not in ancestors:
        q = all_parents[q]
    return q
```

