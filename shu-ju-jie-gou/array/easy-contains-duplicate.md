# \[Easy\] Contains Duplicate / \[Easy\] Contains Duplicate II

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



## [\[Easy\] Contains Duplicate II ](https://leetcode.com/problems/contains-duplicate-ii/)        \(1193/1321\)

### 1. Sliding Window Brute Force: O\(n \* min\(k,n\)\) / O\(1\)

放兩根指針，左指針指的是k的頭，右指針指的是k的尾。  
兩根指針都從左開始往右走，如果左右兩指針left&right滿足下列兩條件：  
\(1\) `right - left == k` \(2\) `nums[left] == nums[right]`，即可返回True，否則返回False。

```python
# 此解法會Time Limit Exceeded
# 並且nums=[99,99] k=2這個case不會過 
def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:

    if not nums or k == 0:
        return False

    for left in range(len(nums)):
        for right in range(left, len(nums)):
            if right - left == k and nums[left] == nums[right]:
                return True
    return False
```

使用Sliding Window解法。  
利用k來維持right & left的sliding window距離，當作是第二個for loop的起點。要注意的是，由於`right - k`有可能`< 0` \(`window_len`距離不可能為負\)，因此我們需要化所有的負距離為0。

```python
def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:

    for right in range(len(nums)):
        # sliding window 
        window_len = max(right - k, 0)
        for left in range(window_len, right):
            if nums[left] == nums[right]:
                return True        
    return False
```

### 2. Sliding Window + Set: O\(N\) / O\(N\)

```python
def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:

    seen = set()
    for idx, num in enumerate(nums):
        # 3. saw it second time
        # meaning that it satisfy the k contract & duplicate conditions
        if num in seen:
            return True            
        # 1. add it to seen set
        seen.add(num)
        # 2. sliding window len k
        # modify size of seen by removing old items
        if len(seen) > k:
            seen.remove(nums[idx - k])
    return False
```

