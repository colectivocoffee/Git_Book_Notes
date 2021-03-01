# \[Easy\] Move Zeros

## [\[Easy\] Move Zeros](https://leetcode.com/problems/move-zeroes/)    \(5115/158\)

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.  
Note:  
1. You must do this **in-place** without making a copy of the array.  
2. Minimize the total number of operations.

```text
Example
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

### 1. Two Pointers \(-&gt;/-&gt;\) and swap: O\(N\) / O\(1\)

```python
def moveZeroes(self, nums: List[int]) -> None:
    """
    Do not return anything, modify nums in-place instead.
    """

    last_zero = 0 
    
    for i in range(len(nums)):
        if nums[i] != 0:
            nums[i], nums[last_zero] = nums[last_zero], nums[i]
            last_zero += 1

    return nums
```

### 2. 先找last non zero idx, 後補0：O\(N\) / O\(1\)

Find the last non zero index first, then place zero onto the remaining elements

```python
e.g. [ 0 , 1 , 0 , 3 , 12 ]
#non-zero      ^
       1 , 3 , 12           # find the last non_zero idx first
      
                    0 , 0   # replacing remaining items with zero
def moveZeroes(self, nums: List[int]) -> None:
    """
    Do not return anything, modify nums in-place instead.
    """
    
    # find the last non_zero idx first
    non_zero = 0
    for num in nums:
        if num != 0:
            nums[non_zero] = num
            non_zero += 1
    
    # replacing remaining items with zero
    for i in range(non_zero, len(nums)):
        nums[i] = 0

    return nums
```



