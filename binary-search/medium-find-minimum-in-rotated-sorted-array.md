# \[Medium\] Find Minimum in Rotated Sorted Array

[Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)  
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.\(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`\).  
Find the minimum element.  
You may assume no duplicate exists in the array.

## Thought Process

### Binary Search

如果題意說是Sorted Array/Rotated Sorted Array，通常這種題目都是可以利用Binary Search來達到O\(logn\)的複雜度。

![\(b\)\(c\) Rotated Sorted Array &#x53EF;&#x80FD;&#x51FA;&#x73FE;&#x60C5;&#x6CC1;](../.gitbook/assets/rotated-sorted-array.jpg)

Time Complexity: O\(logn\)  
Space Complexity: O\(1\)

## Code

#### 1. Binary Search: O\(logn\)/O\(1\)

思路請見Binary Search模板

{% tabs %}
{% tab title="Python" %}
```python
def findMin(self, nums: List[int]) -> int:
    
    if not nums or len(nums) == 0:
        return -1
        
    start, end = 0, len(nums)-1
    
    while start + 1 < end:
        
        mid = start + (end-start)//2
        
        if nums[mid] < nums[end]:
            end = mid
        else:
            start = mid
    
    if nums[start] < nums[end]:
        return nums[start]
    
    return nums[end]
```
{% endtab %}
{% endtabs %}

