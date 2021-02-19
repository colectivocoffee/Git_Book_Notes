# \[Medium\] House Robber / \[Medium\] Robber II

## [\[Medium\] House Robber](https://leetcode.com/problems/house-robber/)         \(6519/185\)



> 思路：  
> step1. 把Robbing的關係搞清楚。Robber在決定是否要搶這房子時，有兩種選擇   
> a\) **Rob current house**: 意即不能搶上一家 \(`i-1`\) &下一家\(`i+1`\)，但可以安全地搶上上一家\(`i-2`\)  
> b\) **Don't rob current house**: 意即上一家\(`i-1`\)以前並沒有任何限制。  
>   
> 因此我們可以發現，目前的搶最大價值可以是取上面a\) or b\)的結果，我們可以化成下面的式子：`# max so far  =  (a)rob last one OR (b)max up to house i-2 + rob current   
>    rob_max[i] = max(rob_max[i-1], rob_max[i-2] + nums[i])`

### 1. Recursive Top-Down: O\(2^N\) / O\(1\)

```python
def rob(self, nums: List[int]) -> int: 

    return self.selectHouse(nums, len(nums)-1)

def selectHouse(self, nums, idx):
    if idx < 0:
        return 0

    return max(self.selectHouse(nums, idx - 2) + nums[idx], self.selectHouse(nums, idx - 1))
```

### 2. Recursive Top-Down + dp Memo: O\(N\) / O\(N\)

```python
def rob(self, nums: List[int]) -> int:

    dp = [-1 for _ in range(len(nums))]
    # 我們只看到最後一個房子，即len(nums)-1。否則會超過
    return self.selectHouse(nums, len(nums)-1, dp)

def selectHouse(self, nums, i, dp):
    # 邊界條件
    if i < 0:
        return 0
        
    # 如果已算過，直接返回dp[i]
    if dp[i] >= 0:
        return dp[i]
        
    # 沒算過，用上面recursion的方法算
    curr_max = max(self.selectHouse(nums, i-1, dp), self.selectHouse(nums, i-2, dp) + nums[i])
    # 紀錄算出來的答案
    dp[i] = curr_max
    return curr_max
```

### 4. Iterative Bottom-Up & Sequence DP: O\(N\) / O\(N\)

```python
def rob(self, nums: List[int]) -> int:

    prev_max, curr_max = 0, 0

    for num in nums:
        temp = curr_max
        curr_max = max(curr_max, prev_max + num)
        prev_max = temp

    return curr_max
```

## \[Medium\] [House Robber II](https://leetcode.com/problems/house-robber-ii/)       \(2611/62\)

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

