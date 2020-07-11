# \[Medium\] Search in Rotated Sorted Array II

Search in Rotated Sorted Array II  
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.  
\(i.e., `[0,0,1,2,2,5,6]` might become `[2,5,6,0,0,1,2]`\).  
You are given a target value to search. If found in the array return `true`, otherwise return `false`.

#### Example

```text
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

## Thought Process

### Binary Search: O\(logn\)/O\(1\)

這題是Search In Rotated Sorted Array的Follow Up Question，即array裡面有可能有重複數字。

那我們要如何處理duplicates呢？

很簡單，在比start target mid end 大小之前，  
先check當`start + 1 < end and nums[index] == nums[index+1]` ，如果符合，把index += 1即可。  
另外，這題要求返回的True/False而不是array。

## Code

{% tabs %}
{% tab title="Python" %}
```python
def search(self, nums: List[int], target: int) -> bool:

    if not nums or len(nums) == 0:
        return False
        
    start, end = 0, len(nums)-1
    
    while start + 1 < end:
        
        # if we see duplicates, then move index. 
        while start + 1 < end and nums[start] == nums[start + 1]:
            start += 1
        while start + 1 < end and nums[end] == nums[end - 1]: #end-1
            end -= 1
            
        mid = start + (end-start)//2
        # mid meets the target
        if nums[mid] == target:
            return True
        
        # four different situations
        if nums[start] < nums[mid]:
            #(1)
            if nums[start] <= target and target <= nums[mid]:
                end = mid
            #(2)
            else:
                start = mid
            
        if nums[mid] < nums[end]:
            #(3)
            if nums[mid] <= target and target <= nums[end]:
                start = mid
            #(4)
            else:
                end = mid
            
    # finalize start/end
    if nums[start] == target or nums[end] == target:
        return True
    
    # cannot find the target
    return False
    
```
{% endtab %}
{% endtabs %}

