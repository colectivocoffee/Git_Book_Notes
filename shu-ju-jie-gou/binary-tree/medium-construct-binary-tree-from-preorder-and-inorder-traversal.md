# \[Medium\] Construct Binary Tree from Preorder & Inorder Traversal

[Construct Binary Tree from Preorder & Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)   
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

思路：Preorder特性 & Inorder特性

> **Preorder特性** -- `root是preorder[0]`   
>    **Inorder特性** -- `root.left都是left subTree` ; `root.right都是right subTree`  
> 因此，我們可以依照下列步驟  
> Step1\) 用preorder\[0\] 先找root，  
> Step2\) 用找到的root值，來反推root\(preorder\) 在inorder裡的root\_index\(inorder\)  
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
    #                    preorder's new left subtree, inorder's new left subtree 
    root.left = self.buildTree(preorder[1:root_index+1], inorder[:root_index])
    # RightSubtree from inorder
    root.right = self.buildTree(preorder[root_index+1:], inorder[root_index+1:])
    
    return root
    
```

```python
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        
        if not preorder:
            return None
        
        savedMap = { inorder[i] : i for i in range(0, len(inorder)) }    
        return self.construct(preorder, 0, len(preorder), inorder, 0, len(inorder), savedMap)
            
    
    
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

