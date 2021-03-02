# \[Medium\] Find First and Last Position of Element in Sorted Array

## [\[Medium\] Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)  \(5053/193\)

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.  
If `target` is not found in the array, return `[-1, -1]`.  
**Follow up:** Could you write an algorithm with `O(log n)` runtime complexity?

下面是幾個需要特別注意的Edge Cases

```python
# Ex1
# nums = [2,2], target = 2
# res = [0,1]

# Ex2
# nums = [], target = 0
# res = [-1,-1]
```

### 1. Binary Search: O\(logN\) / O\(1\)

> 思路：會有重複的數字Duplicates，用Binary Search時要小心。  
> 這題需要用兩次的Binary Search，第一次用來找First\(Left Bound\)，第二次用來找Last\(Right Bound\)  
> 意即`[5,7,7,8,8,10]`, target = 8  
>                        \|      \|  
>                     left/right bound  
> 其中有幾點要注意  
> \(1\) nums\[mid\] == target 的情況，是選擇start還是end？  
>       First要用end = mid，  
>       Last要用start = mid  
> \(2\) 在定義什麼時候選擇start先返回，什麼時候選擇end先返回時，要特別注意。  
>       First要先返回start，Last要先返回end

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
        #          v        把mid當邊界往左推
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
        #          v         把mid當邊界往右推
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

