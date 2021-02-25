# \[Easy\] Merge Sorted Array

## [\[Easy\] Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)           \(3405/5025\)

Given two sorted integer arrays `nums1` and `nums2`, merge `nums2` into `nums1` as one sorted array.  
The number of elements initialized in `nums1` and `nums2` are `m` and `n` respectively. You may assume that `nums1` has a size equal to `m + n` such that it has enough space to hold additional elements from `nums2`.

```text
Ex1
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]

Ex2
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
```

### 1. Merge and Sort: O\(NlogN\) / O\(1\)

剛開始的時候，不懂為什麼已經有`nums1 & nums2`\(代表已有其長度\)，還是要給`m & n`，後來才知道原來`nums1`總是**有`m+n`的額外空間給`nums2`**，因此`m & n`是指在2 merge into 1時，起始放置的位置。

```python
def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
    """
    Do not return anything, modify nums1 in-place instead.
    """

    for i in range(n):
        nums1[i + m] = nums2[i]

    nums1.sort()
```

