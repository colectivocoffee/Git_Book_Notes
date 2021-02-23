# \[Medium\] House Robber / \[Medium\] Robber II / \[Medium\] Paint House

## [\[Medium\] House Robber](https://leetcode.com/problems/house-robber/)         \(6526/185\)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

> 思路：  
> step1. 把Robbing的關係搞清楚。Robber在決定是否要搶這房子時，有兩種選擇   
> a\) **Rob current house**: 意即不能搶上一家 \(`i-1`\) &下一家\(`i+1`\)，但可以安全地搶上上一家\(`i-2`\)  
> b\) **Don't rob current house**: 意即上一家\(`i-1`\)以前並沒有任何限制。  
>   
> 因此我們可以發現，目前的搶最大價值可以是取上面a\) or b\)的結果，我們可以化成下面的式子：`# max so far  =  (a)rob last one OR (b)max up to house i-2 + rob current   
>    rob_max[i] = max(rob_max[i-1], rob_max[i-2] + nums[i])`

### 1. Sequence DP, Recursive Top-Down: O\(2^N\) / O\(1\)

```python
def rob(self, nums: List[int]) -> int: 

    return self.selectHouse(nums, len(nums)-1)

def selectHouse(self, nums, idx):
    if idx < 0:
        return 0

    return max(self.selectHouse(nums, idx - 2) + nums[idx], self.selectHouse(nums, idx - 1))
```

### 2. Sequence DP, Recursive Top-Down + dp Memo: O\(N\) / O\(N\)

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

### 3. Sequence DP, Iterative Bottom-Up + dp memo: O\(N\) / O\(N\)

> 易錯點  
> \(1\)注意“**邊界定義”：**預防index out of range  
> 因為在robbing的時候，在`nums[i]`位置時，會需要看`i-1`和`i-2`的curr\_max \(分別紀錄於`dp[i]` & `dp[i-1]`\)  
> 因此，dp長度需要比len\(nums\)還要多1  
> \(2\)**DP base case：**也要先定義完成

```python
# e.g. [2,1,1,2]
# dp = [0,2,3,3,4]
def rob(self, nums: List[int]) -> int:

    if not nums:
        return 0

    dp = [-1 for _ in range(len(nums)+1)]
    # DP base case
    dp[0] = 0
    dp[1] = nums[0]

    for i in range(1,len(nums)):
        dp[i+1] = max(dp[i], dp[i-1] + nums[i])

    return dp[-1]
```

### 4. Sequence DP, Iterative Bottom-Up + 2 variables: O\(N\) / O\(1\)

此為sliding window版本。  
唯一一點要注意的地方是，`curr_max`不能馬上被更新（三步翻轉法）  
需要用temp儲存起來作為`prev_max`用，等算完後再放回去。

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

### 1. Sequence DP

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

### 2. DP + Sliding Window

## [\[Medium\] Paint House](https://leetcode.com/problems/paint-house/)         \(1133/101\)

There is a row of n houses, where each house can be painted one of three colors: red, blue, or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a `n x 3` cost matrix. For example, `costs[0][0]` is the cost of painting house 0 with the color red; `costs[1][2]` is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

```text
Example
Input: costs = [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue.
Minimum cost: 2 + 5 + 3 = 10.
```

