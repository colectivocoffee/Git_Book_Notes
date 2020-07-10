# \[Medium\] House Robber II

## Question

[House Robber II](https://leetcode.com/problems/house-robber-ii/)  
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

## Thought Process

### \(1\) Sequence DP

Step1

`rob[i]` 表示為搶劫nums\[i\] 可獲得的最大收益，  
`no_rob[i]` 表示為不搶nums\[i\] 可獲得的最大收益。此時**`no_rob[i]` 決定於i-1的最大收益**，即 max\(rob\[i-1\], no\_rob\[i-1\]\)。  
  
以下數組為例：2112  
                     rob 2134  
               no\_rob 0223  
  
最大收益為max\(rob, no\_rob\)找最大值，即4。  
  
Step2  
由於是環形，必須要考慮nums\[i\]左右兩邊start&end的房子，即`i-2 & 0`，還有`i-1 & 1`。  
  
Time Complexity: O\(n\)  
Space Complexity:O\(1\)

### \(2\) DP + Sliding Window

## Full Implementation

{% tabs %}
{% tab title="Python" %}
```python
def rob(self, nums: List[int]) -> int:
    
    if len(nums) == 0:
        return 0
    
    if len(nums) == 1:
        return nums[0]
    
    return max(self.robHelper(nums, 0, len(nums)-2), self.robHelper(nums, 1, len(nums)-1)
    
def robHelper(self, nums, start, end):
    
    rob, no_rob = 0, 0
    for i in range(start, end+1): # range should be (start, end+1) to represent circle
        new_rob = no_rob + nums[i]
        no_rob = max(rob, no_rob)
        rob = new_rob
    
    return max(rob, no_rob)
    
    
```
{% endtab %}
{% endtabs %}

