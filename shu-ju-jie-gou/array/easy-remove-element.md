# \[Easy\] Remove Element

## [Remove Element](https://leetcode.com/problems/remove-element/) \(1868/3259\)

> Given an array _nums_ and a value `val`, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.  
> Do not allocate extra space for another array, you must do this by **modifying the input array** [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) with `O(1)` extra memory.  
> The order of elements can be changed. It doesn't matter what you leave beyond the new length.

### 1. Two pointers \(-&gt;/-&gt;\): O\(N\) / O\(1\)

由於只能使用現有的memory，我們必須要modify the input array in-place。因此我們使用`快慢指針`來完成。  
為什麼用快慢指針呢？因為我們可以讓快指針\(`index(num)`\)往後看，讓num跟val比較；而慢指針\(`idx`\)用來remove element。

```python
def removeElement(self, nums: List[int], val: int) -> int:
    
    idx = 0
    for num in nums:  
#            print(idx, nums[idx], num, nums)
        if num != val:                
            nums[idx] = num
            idx += 1
    
    return idx
```



