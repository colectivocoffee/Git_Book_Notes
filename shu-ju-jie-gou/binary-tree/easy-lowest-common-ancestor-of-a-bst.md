# \[Easy\] Lowest Common Ancestor of a BST

## [\[Easy\] Lowest Common Ancestor of a BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)    \(2247/110\)

Given a binary search tree \(BST\), find the lowest common ancestor \(LCA\) of two given nodes in the BST.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants \(where we allow **a node to be a descendant of itself**\).”

![](../../.gitbook/assets/image%20%2843%29.png)

```text
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

![](../../.gitbook/assets/image%20%2842%29.png)

```text
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

## Code

### 1. DFS Recursion \(same as LCA Binary Tree\)

```python
和Lowest Common Ancestor of a Binary Tree 相同寫法
```

### 2. DFS Recursion \(利用BST特性\):   O\(N\) / O\(N\)

我們從root開始往左右traverse找p&q所在位置。  
第一步是看p&q同時屬於左子樹，同時右子樹，還是p, q各佔一邊。  
第二步，當我們找到p&q後，看LCA是在p本身？還是在q本身？還是在p&q的共同parent？

> 如何找LCA？只有三種情況：  
> \(1\) LCA 在p&q的共同parent      
> \(2\) LCA 在p                                  
> \(3\) LCA 在q   
>   
> \(1\) LCA在p&q的共同parent，代表 `p < LCA < q` or `p > LCA > q`  
>       --&gt; 說明p & q 分別在左右兩顆子樹上  
>       --&gt; 繼續往parent方向找LCA   
>       --&gt; 如果在parent裡找到最小parent \(即LCA\)，則返回當下root node   
> \(2\) \(3\)  如果`root`同時大於`p`和`q`或`root`同时小于`p`和`q`，我們就以下分開討論  
>       2-1. `root`同時大於`p`和`q`：  
>                --&gt; 說明 p 和 q 都在左子樹上  
>                --&gt; 繼續往左子樹找   
>       2-2.  `root`同時小於`p`和`q`，  
>                --&gt; 說明 p 和 q 都在右子樹上  
>                --&gt; 繼續往右子樹找  
>       2-3.   如果root == p or root == q，  
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

