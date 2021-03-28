# \[Medium\] Construct Binary Tree from Preorder & Inorder Traversal

## [\[Medium\] Construct Binary Tree from Preorder & Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)      \(4927/126\)

Given preorder and inorder traversal of a tree, construct the binary tree.  
Note:  
You may assume that duplicates do not exist in the tree.  
For example, given

```text
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```python
    3
   / \
  9  20
    /  \
   15   7
```



## Code

{% hint style="info" %}
\(1\)遞歸出口：用if not preorder處理Null  
\(2\)以Preorder為主，根據`根-左-右`的方式，切掉root，且持續對左子樹&右子樹切片並放入Recursion本身。
{% endhint %}

![The recursive structure in a Tree](../../.gitbook/assets/image%20%2854%29.png)

![&#x5207;&#x5206;&#x524D; -&amp;gt; &#x5207;&#x5206;&#x5F8C;](../../.gitbook/assets/image%20%2848%29.png)

### 1. Recursion + Preorder&Inorder特性: O\(NlogN\)-O\(N^2\) / O\(N\)

思路：Preorder特性 & Inorder特性

> **Preorder特性** -- `root是preorder[0]`   
>    **Inorder特性** -- `root.left都是left subTree` ; `root.right都是right subTree`  
> 因此，我們可以依照下列步驟  
> Step1\) 用preorder\[0\] 先找root，  
> Step2\) 用從preorder找到的root值，來反推root\(preorder\) 在inorder裡的root\_index\(inorder\)  
> Step3\) 用Recursion + 更新的index來持續遍歷整個list，並且用root建立Tree  
> Step4\) return root

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:

    if not preorder:
        return None
    
    # first node from preorder is always the root.    
    root = TreeNode(preorder[0])
    root_index = inorder.index(preorder[0])
    
    # LeftSubtree from inorder
    #                    (preorder's new left subtree, inorder's new left subtree) 
    root.left = self.buildTree(preorder[1:root_index+1], inorder[:root_index])
    # RightSubtree from inorder
    root.right = self.buildTree(preorder[root_index+1:], inorder[root_index+1:])
    
    return root
    
```

### 2. Recursion + Preorder&Inorder特性 Improved:    O\(N\) / O\(N\)

由於上面的解法，在每一次的Recursion call都需要一直對preorder & inorder list做 index slicing\(切片\)。我們如果事先把index存起來，就可以大大地減少在index slicing上的計算量。\(但是這個解法在recursion上會需要傳很多的parameters\) 

> 傳 preorder&inorder的index。  
> 實時更新 preStart / preEnd / inStart / inEnd

```python
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        
        if not preorder:
            return None
        
        savedMap = { inorder[i] : i for i in range(0, len(inorder)) }    
        return self.construct(preorder, 0, len(preorder), inorder, 0, len(inorder), savedMap)
            
    # TODO: reuse savedMap
    def construct(self, preorder, preStart, preEnd, inorder, inStart, inEnd, savedMap):
        
        if preStart >= preEnd or inStart >= inEnd:
            return None
        
        root = TreeNode(preorder[preStart])
        leftTree = inorder.index(preorder[preStart]) - inStart
        
#        print(root, preStart, inStart)
        
        root.left = self.construct(preorder, preStart + 1, preStart + leftTree + 1, inorder, inStart, inStart + leftTree, savedMap)
        root.right = self.construct(preorder, preStart + leftTree + 1, preEnd, inorder, inStart + leftTree + 1, inEnd, savedMap)
        
        return root
```

