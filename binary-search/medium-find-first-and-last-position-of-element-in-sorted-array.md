# \[Medium\] Find First and Last Position of Element in Sorted Array

## [\[Medium\] Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)  \(\)

### 1. Binary Search: O\(logN\) / O\(1\)

```python
def searchRange(self, nums: List[int], target: int) -> List[int]:

    result = [-1, -1]

    if not nums:
        return result

    result[0] = self.findFirst(nums, target)
    result[1] = self.findLast(nums, target)
    return result


def findFirst(self, nums, target):

    start, end = 0, len(nums)-1

    while start + 1 < end:
        mid = start + (end - start) // 2     
        if nums[mid] < target:
            start = mid        
        elif nums[mid] > target:
            end = mid
        
        #         mid
        # [start, ..., end]
        #          v        把mid邊界往左推
        # [start, end, end]
        # left bound when nums[mid] == target
        else:
            end = mid
    
    # [start,end]
    #    ^    
    # 正常版=> 先看start(左邊界)，再看end(右邊界)
    if nums[start] == target: 
        return start
    if nums[end] == target:
        return end
    return -1

def findLast(self, nums, target):

    start, end = 0, len(nums)-1

    while start + 1 < end:
        mid = start + (end - start) // 2
        if nums[mid] < target:
            start = mid
        elif nums[mid] > target:
            end = mid
        
        #         mid
        # [start, ..., end]
        #          v         把mid邊界往右推
        # [start,start, end]
        # right bound when nums[mid] == target
        else:
            start = mid
    
    # 易錯點： 
    # [start,end]
    #         ^
    # 先看end(右邊界)，再看start(左邊界)
    if nums[end] == target:
        return end
    if nums[start] == target:
        return start
    return -1
```

