# \[Easy\] Search Insert Position

[Search Insert Position](https://leetcode.com/problems/search-insert-position/)  
Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.  
You may assume no duplicates in the array.

## Thought Process

### Binary Search

跟Template沒什麼不同，唯一要判斷的是最後要放在哪個位置。

```python
[....(start)....(end)....]
  (1)  (2)  (3)  (4)  (5)
   
#(2)
if nums[start] == target:
    return start
#(4)
if nums[end] == target:
    return end

# (1)
if nums[start] > target:
    return start
# (3)
elif nums[start] < target < nums[end]:
    return end
# (5)
elif nums[end] < target:
    return end + 1
# none of above
return 0
```

## Code

```python
def searchInsert(self, nums: List[int], target: int) -> int:

    if not nums:
        return 0
        
    start, end = 0, len(nums)-1
    while start + 1 < end:
        
        mid = start + (end-start)//2
        if nums[mid] == target:
            return mid
        elif nums[mid] > target:
            end = mid
        else: # mid < target
            start = mid
     
     if nums[start] == target:
         return start
     if nums[end] == target:
         return end
     
     if nums[start] > target:
         return start
     elif nums[start] < target < nums[end]:
         return end
     elif nums[end] < target:
         return end + 1
     return 0 
```

