# \[Medium\] Minimum Size Subarray Sum

[Minimum Size Subarray Sum  
](https://leetcode.com/problems/minimum-size-subarray-sum/)Given an array of **n** positive integers and a positive integer **s**, find the minimal length of a **contiguous** subarray of which the sum ≥ **s**. If there isn't one, return 0 instead.

#### Example

```text
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
```

## Thought Process

### 1. Sliding Window\(-&gt;/-&gt;\): O\(n\)/O\(n\)

Sliding Window template的簡易版。唯一需要注意的地方是要如何'update window'。原來update window的方法是  
`char = s[right]  
window[char] = window.get(char,0)+1  
right += 1`  
然而，在這裡不需要這麼複雜，我們先right +=1，並且即時update sub\_sum即可，這裡的sub\_sum就是前面的window。  
`right += 1  
sub_sum = sum(nums[left:right])`

### 2. Binary Search: O\(logn\)

## Code

#### 1. Sliding Window

{% tabs %}
{% tab title="Python" %}
```python
def minSubArrayLen(self, s: int, nums: List[int]) -> int:
    
    
    left,right = 0,0
    sub_sum = 0
    min_len = float('inf')
    
    while right < len(nums):
        #1. move right pointer
        right += 1
        sub_sum = sum(nums[left:right])
        
        #2. shrink window (in the condition of sub_sum is still greater than s)
        while sub_sum >= s:
            #4. update result
            curr_len = right-left
            if curr_len < min_len:
                min_len = curr_len
            #3. move left pointer
            left += 1
            sub_sum = sum(nums[left:right])
    
    if min_len == float('inf'):
        return 0
    return min_len
```
{% endtab %}
{% endtabs %}

