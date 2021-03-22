# \[Medium\] Lowest Common Ancestor of a Binary Tree / \[Easy\] LCA of a BST

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

## [\[Easy\] Lowest Common Ancestor of a BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)    \(2247/110\)

Given a binary search tree \(BST\), find the lowest common ancestor \(LCA\) of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants \(where we allow **a node to be a descendant of itself**\).”



![](https://gblobscdn.gitbook.com/assets%2F-M5JC8imwSUnef3zjvhP%2F-MWLQ9-l-UgYwZn-wdRK%2F-MWQCEbVP65ywCWwLRhP%2Fimage.png?alt=media&token=5c90eb76-c9a9-4d0e-a390-270482a3ba57)

```text
Ex1
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

```text
Ex2
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

The below diagram would help in understanding what `lowest` means. Following cases are possible:

![](../../.gitbook/assets/image%20%2843%29.png)

### 1. DFS Recursive \(same as LCA Binary Tree\): O\(N\) / O\(N\)

```python
和Lowest Common Ancestor of a Binary Tree 相同寫法
```

### 2. DFS Recursive \(利用BST特性\):   O\(N\) / O\(N\)

我們從root開始往左右traverse找p&q所在位置。  
第一步是看p&q同時屬於左子樹，同時右子樹，還是p, q各佔一邊。  
第二步，當我們找到p&q後，看LCA是在p本身？還是在q本身？還是在p&q的共同parent？

> 如何找LCA？只有三種情況：  
> \(case1\) LCA 在p&q的共同parent      
> \(case2\) LCA 在p                                  
> \(case3\) LCA 在q   
>   
> \(case1\) LCA在p&q的共同parent，代表 `p < LCA < q` or `p > LCA > q`  
>       --&gt; 說明p & q 分別在左右兩顆子樹上  
>       --&gt; 繼續往parent方向找LCA   
>       --&gt; 如果在parent裡找到最小parent \(即LCA\)，則返回當下root node   
> \(case2\) \(case3\)  如果`root`同時大於`p`和`q`或`root`同时小于`p`和`q`，我們就以下分開討論  
>       step1. `root`同時大於`p`和`q`：  
>                --&gt; 說明 p 和 q 都在左子樹上  
>                --&gt; 繼續往左子樹找   
>       step2.  `root`同時小於`p`和`q`，  
>                --&gt; 說明 p 和 q 都在右子樹上  
>                --&gt; 繼續往右子樹找  
>       step3.   如果root == p or root == q，  
>                 --&gt; 看先遇見p還是q，就直接返回第一個\(當下的root\)

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
    
    pVal = p.val
    qVal = q.val
    
    if not root:
        return root
        
    # BST特性：                                  中(root)
    # 左子樹的所有值一定都小於root.val，          /           \
    # 右子樹的所有值一定都大於root.val           小           大 
    #                                      /   \        /   \
    #                                    最小  小中    中大   最大
    if pVal < root.val and qVal < root.val:
        return self.dfs(root.left, p, q)
    elif pVal > root.val and qVal > root.val:
        return self.dfs(root.right, p, q)
    else:
        return root    
```

