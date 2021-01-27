# \[Medium\] Rotate Array

## \*\*\*\*[**Rotate Array**](https://leetcode.com/problems/rotate-array/)  **\(4048/899\)**

Given an array, rotate the array to the right by _k_ steps, where _k_ is non-negative.

**Follow up:**

* Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
* Could you do it in-place with O\(1\) extra space?

```python
Example 1:
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]


Example 2:
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

### 1. Python Slice: O\(N\)/O\(1\)

```python
# e.g. nums = [1,2,3,4,5,6,7], k = 10
# k remainder = 3 
# slice ->    [1,2,3,4|5,6,7]
#               left  | right 
def rotate(self, nums: List[int], k: int) -> None:
    """
    Do not return anything, modify nums in-place instead.
    """
    
    
    # k = k % len(nums)
    # k remainder = 3     
    k %= len(nums) 
    
    left = nums[:len(nums)-k]    # left = [1,2,3,4] 
    right = nums[len(nums)-k:]   # right = [5,6,7]
    
    nums[:] = right + left
```

or even better

```python
def rotate(self, nums: List[int], k: int) -> None:
    
    k %= len(nums)
    
    nums[:] = nums[len(nums)-k:] + nums[:len(nums)-k]
```

