# \[Medium\] Product of Array Except Self

[Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)  
Given an array `nums` of _n_ integers where _n_ &gt; 1, return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

## Thought Process

## Code

#### 1. DP: O\(n\)/O\(n\)

{% tabs %}
{% tab title="Python" %}
```python
def productExceptSelf(self, nums: List[int]) -> List[int]:
    
    if not nums or len(nums) == 0:
        return 0
        
    f = [0 for i in range(len(nums))]
    
    # need 1 instead of 0, because 1*x = x
    firstHalf = [1 for i in range(len(nums))]
    # be extra cautious about the range, since i=i-1 could cause out of range.
    for i in range(1, len(nums)):
        # transfer function: f[i] = f[i-1] + nums[i-1]
        firstHalf[i] = firstHalf[i-1] + nums[i-1]
        
    secondHalf = [1 for i in range(len(nums))]
    # range(0, len(nums)) for i = i+1
    for i in range(0, len(nums)):
        # transfer function: f[i] = f[i+1] + nums[i+1]
        secondhalf[i] = secondHalf[i+1] + nums[i+1]
    
    for i in range(len(nums)):
        f[i] = firstHalf[i] * secondHalf[i]
        
    return f
    
```
{% endtab %}
{% endtabs %}

#### 2. DP Space Optimized: O\(n\)/O\(1\)

