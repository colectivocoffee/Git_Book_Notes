# \[Easy\] Convert Sorted Array to Binary Search Tree

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

### 1. Binary Search:    O\(N\) / O\(N\)

Time Complexity:  O\(N\)  
Space Complexity:  O\(N\), O\(N\) to keep the output, and O\(logN\) for the recursion stack.

如果要建Binary Tree / Binary Search Tree，我們依次建立`root/root.left/root.right` 之間的三角關係。然後一直往下走，直到nums全部看完為止。

```python
def sortedArrayToBST(self, nums: List[int]) -> TreeNode:

    def convert(left, right):
        if left > right:
            return

        curr = left + (right - left) // 2

        # 建立 root, root.left, root.right 的相互關係
        root = TreeNode(nums[curr])
        # 往左走
        root.left = convert(left, curr - 1)
        # 往右走
        root.right = convert(curr + 1, right)
        return root

    return convert(0, len(nums) - 1)
```

