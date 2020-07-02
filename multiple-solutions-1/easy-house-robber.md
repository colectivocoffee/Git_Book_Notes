# \[Easy\] House Robber

## Question

## Thought Process

### \(1\) Recursion\(Top-Down\)

### \(2\) DP

### \(3\) DP + Sliding Window



## Full Implementation

#### \(1\) Recursion \(Top-down\)

This solution will cause Time Limit Exceeded\(TLE\).

{% tabs %}
{% tab title="Python" %}
```python
def rob(self, nums: List[int]) -> int:
    return self.robbing(nums, len(nums)-1)
    
def robbing(self, nums, i):
    
    if i < 0:
        return 0
        
    return max(self.robbing(nums, i-2) + nums[i], self.robbing(nums, i-1))
  
```
{% endtab %}
{% endtabs %}

#### \(2\) DP

{% tabs %}
{% tab title="Python" %}
```python
def rob(self, nums: List[int]) -> int:

    if len(nums) == 0:
        return 0
        
    if len(nums) == 1:
        return nums[0]
        
    rob, no_rob = 0, 0
    for i in range(len(nums)):
        new_rob = no_rob + nums[i]
        no_rob = max(rob, no_rob)
        rob = new_rob
    
    return max(rob, no_rob)
            
    
```
{% endtab %}
{% endtabs %}



