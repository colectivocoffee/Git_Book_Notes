# \[Medium\] Next Permutation

[Next Permutation](https://leetcode.com/problems/next-permutation/)  
Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order \(ie, sorted in ascending order\).

The replacement must be [**in-place**](http://en.wikipedia.org/wiki/In-place_algorithm) and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

`1,2,3` → `1,3,2`  
`3,2,1` → `1,2,3`  
`1,1,5` → `1,5,1`

## Thought Process

## Code

```python
def nextPermutation(self, nums: List[int]) -> None:
    if not nums:
        return
        
    left = -1            # to accomodate case like [4,3,2,1] that needs to be reversed entirely.
    right = len(nums)-1
    
    # going backwards
    # [2,1,4,3]
    #    <-- r    
    while right > 0:
        # found first element(nums[right-1]) where it is smaller than prev one(nums[right])
        if nums[right - 1] < nums[right]:
            # mark the first element index using left pointer
            left = right-1
            break
        right -= 1
    
    # swap everything from the end to the first element
    for right in range(len(nums))[::-1]:
        # all elements after first element
        if nums[right] > nums[left]:
            # then swap
            nums[right], nums[left] = nums[left], nums[right]
            # then sort the remaning ones into ascending order
            nums[left+1:] = sorted(nums[left+1:])
            return
    
```

