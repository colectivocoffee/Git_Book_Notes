# \[Hard\] Longest Consecutive Sequence

## [\[Hard\] Longest Consecutive Sequence ](https://leetcode.com/problems/longest-consecutive-sequence/)     \(4695/229\)

Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

**Follow up:** Could you implement the `O(n)` solution?

```text
Example1
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. 
Therefore its length is 4.

Example2
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9
```

### 1. Set + Sorted: O\(nlogn\) / O\(n\)

```python
def longestConsecutive(self, nums: List[int]) -> int:

    if not nums:
        return 0

    nums = set(nums)
    nums = sorted(nums)
    count = 1
    longest = 1

    for i in range(1, len(nums)):
        if nums[i] != nums[i-1]:
            if nums[i] == nums[i-1] + 1:
                count += 1
            else:
                longest = max(longest, count)
                count = 1
#            print(i, longest, count)                
#    print(longest, count)
    
    return max(longest, count)
```

### 2. Union Find: O\(n\)

這種解法可以解決Follow Up 的 O\(n\)要求

