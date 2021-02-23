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

### 1. Brute Force: O\( $$2^n$$ \) or O\( $$3^n$$ \) / O\(N\)-O\( $$n 3^n$$ \)

Step1. 找出所有可能的組合\(all possible permutations\)，並計算出所有的花費，這組合必須要符合**相鄰不同色**的條件。  
Step2. 找出花費最低的選擇，結束。

How to get all possible permutations?  
To generate every possible length-n string of `0`, `1`, and `2`, remove any that have the same digit twice in a row, and then score those that are left, keeping track of the smallest cost seen so far.

Time Complexity: O\( $$2^n$$ \) 如果用上面的方法產生all permutations，就是2^n。The number of valid permutations doubles with every house added. 如下例，4 houses will  
have 24 permutations.

```text
house  color   num of permutations
   1 [ , , ]     3
   2 [ , , ]     6
   3 [ , , ]     12
   4 [ , , ]     24
```

Space Complexity: O\(N\)-O\( $$n 3^n$$ \)  number of permutations

![](../.gitbook/assets/image%20%2819%29.png)

![All Possible Permutations](../.gitbook/assets/image%20%2822%29.png)

以4 houses為例，下為24種permutations的產生法：

![](../.gitbook/assets/image%20%2820%29.png)

### 2. Brute Force, Recursive Top-Down: O\(2^n\) / O\(n\)

由Brute Force approach1可以發現，在選擇房子顏色時，可以用Recursion來計算所有permutations的總花費\(total\)。

![](../.gitbook/assets/image%20%2818%29.png)

```python
def minCost(self, costs: List[List[int]]) -> int:

    if not costs or len(costs) == 0:
        return 0
    
    # 從 n -> n+1 代表我們從第0房/層往下走，
    # 一層一層去算paint_cost的各種組合最小。
    def paint_cost(n, color):
        total = costs[n][color]
        # 遞歸出口
        if n == len(costs) - 1:
            pass
        elif color == 0:      # red
            total += min(paint_cost(n+1, 1), paint_cost(n+1, 2))
        elif color == 1:      # green
            total += min(paint_cost(n+1, 0), paint_cost(n+1, 2))
        else:                 # blue
            total += min(paint_cost(n+1, 0), paint_cost(n+1, 1))
        return total        


    return min(paint_cost(0,0), paint_cost(0,1), paint_cost(0,2))

# Time Limit Exceeded solution (TLE)
```

### 3. Recursive + Memoization: O\(N\) / O\(N\)

其實經由approach2，做memoization的小小更動，把已經算過的total cost按照 `(n,color) : total_cost`的方式紀錄在dictionary，就可以大大減少重複計算。  
我們可以把Time Complexity降至O\(N\)

Time Complexity: O\(N\)  There are `3 * n` different possible sets of parameters, because there are `n` houses and `3` colors.   
Space Complexity: O\(N\)  

```python
def minCost(self, costs: List[List[int]]) -> int:

    if not costs or len(costs) == 0:
        return 0
    
    # 加上memo
    self.memo = {}

    def paint_cost(n, color):
        #從memo找答案
        if (n,color) in self.memo:
            return self.memo[(n,color)]

        total = costs[n][color]
        if n == len(costs) - 1:
            pass
        elif color == 0:      # red
            total += min(paint_cost(n+1, 1), paint_cost(n+1, 2))
        elif color == 1:      # green
            total += min(paint_cost(n+1, 0), paint_cost(n+1, 2))
        else:                 # blue
            total += min(paint_cost(n+1, 0), paint_cost(n+1, 1))
        # 把計算結果紀錄下來
        self.memo[(n, color)] = total
        return total        


    return min(paint_cost(0,0), paint_cost(0,1), paint_cost(0,2))
```

以上都是用Recursive的方式求解；下面用Iterative的方式求解。

### 4. Sequence DP, Iterative Bottom-Up: O\(N\) / O\(1\)

We'll define a subproblem to be calculating the total cost for a particular house position and color.

![](../.gitbook/assets/image%20%2821%29.png)

![DP Bottom-Up&#x7531;&#x6700;&#x5F8C;&#x4E00;&#x5C64;&#x5F80;&#x4E0A;&#x52A0;&#x7684;&#x904E;&#x7A0B;](../.gitbook/assets/image%20%2824%29.png)

We can, therefore, calculate the cost of each subproblem, starting from the ones with the highest house numbers, and write the results directly into the input array. In effect, we will replace each single-house cost value in the array with the cost of painting the house that color and the minimum cost to paint all the houses after it. This is almost the same as what we did on the tree. The only difference is that we are only doing each calculation once and we are writing results directly into the input table. It is bottom-up, because we are solving the "lower" problems first, and then the "higher" ones once we've solved all the lower ones that they depend on.

為什麼是Bottom-Up？因為如果從最後一個房子來看，後面就沒有其他房子需要注意，因此`total_cost(3,0)` `total_cost(3,1)` `total_cost(3,2)` 的值即為最後一個房子油漆所需花費。我們可以從最後一層慢慢往上走，並且把下面的總花費往上一層加。直到第一層，第0層`total_costs[0]`即為最小花費。

![&#x6700;&#x7D42;&#x7D50;&#x679C; min\(costs\[0\]\), where costs\[0\] = \[34,17,32\]](../.gitbook/assets/image%20%2823%29.png)

從尾往頭掃 `reversed` + `n -> n+1`

```python
def minCost(self, costs: List[List[int]]) -> int:

    if len(costs) == 0:
        return 0
    
    # 層數 3 -> 2 -> 1 -> 0
    # 為什麼層數是n -> n+1？
    # 因為是把下一層[n+1]的結果往上一層[n]加。
    for n in range(len(costs)-1)[::-1]:
        # red
        costs[n][0] += min(costs[n+1][1], costs[n+1][2])
        # green
        costs[n][1] += min(costs[n+1][0], costs[n+1][2])
        # blue
        costs[n][2] += min(costs[n+1][1], costs[n+1][0])

    # costs[0] = [34,17,32]
    # min(costs[0]) = 17    <- ans
    return min(costs[0])
```

從頭到尾掃（但要反著看，`n -> n-1`，因此最後一層`total_costs[-1]`應為最終結果）

```python
def minCost(self, costs: List[List[int]]) -> int:

    if len(costs) == 0:
        return 0
        
    # 同理，是把上一層[n-1]的結果往下一層[n]加。
    for n in range(1, len(costs)):
        # red
        costs[n][0] += min(costs[n-1][1], costs[n-1][2])
        # green
        costs[n][1] += min(costs[n-1][0], costs[n-1][2])
        # blue
        costs[n][2] += min(costs[n-1][1], costs[n-1][0])

    return min(costs[-1])
```

True Time Complexity: O\(n\*m\)  
Finding the minimum of two values and adding it to another value is an O\(1\)operation. We are doing these O\(1\) operations for $$3 \cdot (n - 1)$$cells in the grid. Expanding that out, we get $$3 \cdot n - 3$$. The constants don't matter in big-oh notation, so we drop them, leaving us with O\(n\).

_A word of warning:_ This would _not_ be correct if there were mm colors. For this particular problem we were told there's only 3 colors. However, a logical follow-up question would be to make the code work for any number of colors. In that case, the time complexity would actually be $$O(n \cdot m)$$, because m is not a constant, whereas 3 is. 

### 5. Sequence DP, Iterative, not modifying input array: O\(N\) / O\(1\)

用`prev_row`,`curr_row`來紀錄上一層, 當層的所有costs\[i\]。  
先`copy.deepcopy(costs[i])`當作是`curr_row`，  
當完成後把`curr_row`assign給`prev_row`。

```python
def minCost(self, costs: List[List[int]]) -> int:

    if len(costs) == 0:
        return 0

    prev_row = costs[-1]
    for n in range(len(costs)-1)[::-1]:
        # 深度拷貝costs[n]，成為curr_row
        curr_row = copy.deepcopy(costs[n])
        # red
        curr_row[0] += min(prev_row[1], prev_row[2])
        # green
        curr_row[1] += min(prev_row[0], prev_row[2])
        # blue
        curr_row[2] += min(prev_row[1], prev_row[0])
        
        # assign curr_row 成為上一層 prev_row
        prev_row = curr_row

    return min(prev_row)
```

