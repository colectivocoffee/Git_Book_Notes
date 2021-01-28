# \[Hard\] First Missing Positive

## [\[Hard\] First Missing Positive  \(5063/922\)](https://leetcode.com/problems/first-missing-positive/)

Given an unsorted integer array `nums`, find the smallest missing positive integer.

**Follow up:** Could you implement an algorithm that runs in `O(n)` time and uses constant extra space?

需要想到的edge cases：

* `[]`
* `[1]`
* `[0]`
* `[7,8,9,11,12]`

### 1. `Sorted()` + `smallest = 1`: O\(NlogN\) / O\(1\)

```python
def firstMissingPositive(self, nums: List[int]) -> int:
    
    nums = sorted(nums)
    # 易錯點：
    # smallest在update之前一直會是前一個數。
    # 並且按題意要求，如果最小的數都比1大 or nums為空，則返回1       
    smallest = 1
    
    for num in nums:
        if num == smallest: 
            smallest += 1        
    return smallest 
```

### 2. 

