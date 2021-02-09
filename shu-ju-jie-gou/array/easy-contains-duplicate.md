# \[Easy\] Contains Duplicate

## [\[Easy\] Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)   \(1433/825\)

Given an array of integers, find if the array contains any duplicates.  
Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.

### 1. Sort: O\(NlogN\) / O\(N\)

```python
def containsDuplicate(self, nums: List[int]) -> bool:

    nums = sorted(nums)
    # 易錯點：範圍是(1,len(nums))，這樣才能在一開始看到nums[i-1]
    for i in range(1,len(nums)):
        if nums[i] == nums[i-1]:
            return True
    return False
```

### 2. Dictionary/HashMap: O\(N\) / O\(N\)

```python
def containsDuplicate(self, nums: List[int]) -> bool:

    duplicates = {}
    for num in nums:
        duplicates[num] = duplicates.get(num, 0) + 1

    for num, count in duplicates.items():
        if count > 1:
            return True
    return False
```

### 3. Set: O\(N\) / O\(N\)

```python
def containsDuplicate(self, nums: List[int]) -> bool:

    return len(nums) != len(set(nums))
```



