# \[Easy\] Convert Sorted Array -&gt; BST / \[Medium\] Convert Linked List -&gt; BST

## [\[Easy\] Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)         \(3610/267\)

Given an integer array `nums` where the elements are sorted in **ascending order**, convert _it to a **height-balanced** binary search tree_.

A **height-balanced** binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.  


![](../../.gitbook/assets/image%20%2844%29.png)



![](../../.gitbook/assets/image%20%2846%29.png)

```text
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted
```

### 1. DFS Recursive, Preorder:    O\(N\) / O\(N\)

Time Complexity:  O\(N\)  
Space Complexity:  O\(N\), O\(N\) to keep the output, and O\(logN\) for the recursion stack.

> 思路：  
> BST特性 --- 從左 -&gt; root -&gt; 右為遞增，因此**root為整個nums中點**。  
> 因此要先用 `middle = len(nums) // 2` 取得 `root`

如果要建Binary Tree / Binary Search Tree，我們依次建立`root/root.left/root.right` 之間的三角關係。然後一直往下走，直到nums全部看完為止。

```python
def sortedArrayToBST(self, nums: List[int]) -> TreeNode:

    def convert(left, right):
        # 遞歸出口
        if left > right:
            return
        
        # 取中間(靠左)作為根
        # always choose left middle node as a root
        curr = left + (right - left) // 2

        # 建立 root, root.left, root.right 的相互關係
        # Preorder 根-左-右
        root = TreeNode(nums[curr])
        # 左
        root.left = convert(left, curr - 1)
        # 右
        root.right = convert(curr + 1, right)
        
        return root
    return convert(0, len(nums) - 1)
```

### 2. DFS Recursive, Preorder + Slicing:   O\(N\) / O\(N\)

同樣思路，但更簡潔的寫法。

```python
def sortedArrayToBST(self, nums: List[int]) -> TreeNode:

    if not nums:
        return 
    
    # nums的中點為BST root 
    middle = len(nums)//2
    
    # Preorder: 根-左-右
    root = TreeNode(nums[middle])
    root.left = self.sortedArrayToBST(nums[:middle])
    root.right = self.sortedArrayToBST(nums[middle + 1:])

    return root
```



## [\[Medium\] Convert Sorted List to BST ](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)     \(2758/94\)

Given the `head` of a singly linked list where elements are **sorted in ascending order**, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of _every_ node never differs by more than 1.

![](../../.gitbook/assets/image%20%2849%29.png)

```text
Input: head = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.
```

### 1. DFS Recursive, Preorder + Binary Search

```python
# Utitlity method 1 to convert original linked list -> array
def convert(self, head):
    converted_list = []
    while head:
        converted_list.append(head.val)
        head = head.next
    return converted_list

# Utility method 2 to convert array -> BST
def listToBST(self, converted, start, end):

    if start > end:
        return

    middle = start + (end - start) // 2
    curr = TreeNode(converted[middle])
    if start == end:
        return curr

    curr.left = self.listToBST(converted, start, middle - 1)
    curr.right = self.listToBST(converted, middle + 1, end)
    return curr

# linked list --> array -binary search--> BST
def sortedListToBST(self, head: ListNode) -> TreeNode:

    if not head:
        return None

    converted = self.convert(head)
    root = self.listToBST(converted, 0, len(converted) - 1)

    return root
```

