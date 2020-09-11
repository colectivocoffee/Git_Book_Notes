# \[Easy\] Closest Binary Search Tree Value

### \*\*\*\*[**Closest Binary Search Tree Value**](https://www.lintcode.com/problem/closest-binary-search-tree-value/description)\*\*\*\*

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

* Given target value is a floating point.
* You are guaranteed to have only one unique value in the BST that is closest to the target.

### **Example**

```text
Input: root = {5,4,9,2,#,8,10} and target = 6.124780
Output: 5
Explanation：
Binary tree {5,4,9,2,#,8,10},  denote the following structure:
        5
       / \
     4    9
    /    / \
   2    8  10
```

### Thought Process

1. 設兩個variable: `upper` and `lower`，起始點都是node. 即 upper = node, lower = node  
2. BST總是`升序排列` ，可以利用此特性跟target做比較．意即：

```text
       [5]  
       / \ 
      4   9

node.val < target  -> move to node.right 
node.val > target  -> move to node.left 
在upper&lower中間則是 
   return node.val
```

 3.. 最後，我們找到upper和lower分別為兩個node, 我們要比較哪個比較靠近target

### Code

{% tabs %}
{% tab title="Python" %}
```python
def closestValue(self, root, target):
        # write your code here
        
        if root is None:
            return None
            
        upper = root
        lower = root
        while root:
            if root.val < target:
                upper = root
                root = root.right
                
            elif root.val > target:
                lower = root
                root = root.left
                
            else: 
                return root.val
            
        
        if abs(upper.val - target) > abs(lower.val - target):
            return lower.val
        return upper.val
```
{% endtab %}
{% endtabs %}

Reference:  
[https://leetcode.com/problems/binary-tree-maximum-path-sum/discuss/603423/Python-Recursion-stack-thinking-process-diagram](https://leetcode.com/problems/binary-tree-maximum-path-sum/discuss/603423/Python-Recursion-stack-thinking-process-diagram)

