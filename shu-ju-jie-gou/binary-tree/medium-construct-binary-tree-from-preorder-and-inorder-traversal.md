# \[Medium\] Construct Binary Tree from Preorder & Inorder Traversal / \[Medium\] Inorder & Postorder

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

![The recursive structure in a Tree](../../.gitbook/assets/image%20%2855%29.png)

![&#x5207;&#x5206;&#x524D; -&amp;gt; &#x5207;&#x5206;&#x5F8C;](../../.gitbook/assets/image%20%2848%29.png)

### 1. Recursion, 根左右特性: O\(NlogN\)-O\(N^2\) / O\(N\)

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

### 2. Recursion, 根左右特性 + Dictionary:    O\(N\) / O\(N\)

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

## [\[Medium\] Construct Binary Tree from Inorder & Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)  \(2537/48\)

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return _the binary tree_.

```text
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]

Then return the following binary tree
    3
   / \
  9  20
    /  \
   15   7
```

![use inorder traversal&apos;s index as root\_idx](../../.gitbook/assets/screen-shot-2021-03-29-at-1.52.03-pm.png)

![](../../.gitbook/assets/image%20%2854%29.png)

### 1. Recursion + Slicing:  O\(NlogN\)-O\(N^2\) / O\(N\)

> Postorder: `左-右-根`，Bottom-&gt;Top，由此可知最後一個必定為root  
> 利用Postorder從後往前推的方式，以postorder\[-1\]為基準點，由後往前一個一個消掉並建成binary tree。  
>   
> ---  
> Postorder traversal: Left -&gt; Right \(usually\), Bottom -&gt; Top.  
> But are going through the postorder list in the **reverse direction**. So, we meet nodes: Left &lt;- Right, Bottom &lt;- Top.

Time Complexity: `O(NlogN)-O(N^2)`, using master theorem, for balanced tree O\(NlogN\), skewed tree O\(N^2\)  
Space Complexity: O\(N\) we store the entire tree

```python
# recommend
def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:

    if not inorder or not postorder:
        return None

    num = postorder.pop()                         # .pop() takes another O(n)
    root = TreeNode(num)
    root_idx = inorder.index(num) 
    # 先右後左, reverse order -> 根->右->左
    # postorder: bottom->top, left->right 左右根
    root.right = self.buildTree(inorder[root_idx+1:], postorder)    #slicing O(n)
    root.left = self.buildTree(inorder[:root_idx], postorder)       

    return root
```

or

```python
def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
    # step0. Recursion exit
    if not inorder or not postorder:
        return None
    # step1. build current root, and get root_idx by postorder->inorder
    num = postorder[-1]
    root = TreeNode(num)        
    root_idx = inorder.index(num)
    
    root.left = self.buildTree(inorder[:root_idx], postorder[:root_idx])
    root.right = self.buildTree(inorder[root_idx+1:], postorder[root_idx:-1])

    return root
```

### 2. Recursion + Dictionary:  O\(N\) / O\(N\)

第一種解法雖然簡潔，但Time & Space Complexity卻不是最好，這種情況下，面試官就會問：Can you optimize your code?

這時候，我們就可以用`Dictionary/Hashmap`來儲存value, index的inorder traversal對應值。只要先inorder traversal一遍後，就不用一直像第一種辦法，切切切很多遍。

```python
def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
    
    # 先inorder traversal一遍，把相對應的idx, val記錄下來
    # idx --> root_idx
    # val --> inorder item value
    inorder_map = {}
    for idx, val in enumerate(inorder):
        # 我們要藉由item value找root_idx，因此是以val為dictionary's key。
        inorder_map[val] = idx

    def build(start, end):

        # recursion exit
        if start > end:
            return None

        num = postorder.pop()
        root = TreeNode(num)
        root_idx = inorder_map[num]
        # 先右後左
        root.right = build(root_idx+1,end)
        root.left = build(start,root_idx-1)
        return root

    return build(0, len(inorder)-1)
```

#### Reference

[https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/discuss/221681/Don't-use-top-voted-Python-solution-for-interview-here-is-why](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/discuss/221681/Don't-use-top-voted-Python-solution-for-interview-here-is-why)

