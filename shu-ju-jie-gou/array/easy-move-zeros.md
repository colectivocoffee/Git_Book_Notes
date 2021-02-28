# \[Easy\] Move Zeros



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

### 2. 

Find the last non zero index first, then redo it again

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



