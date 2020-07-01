# \[Medium\] Coin Change

### Question

\[Medium\]3939/134  
You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

### Example

**Example 1:**

```text
Input: coins = [1, 2, 5], amount = 11
Output: 3 
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**

```text
Input: coins = [2], amount = 3
Output: -1
```

### Thought Process

1. **Define the state 定義狀態**： We don't know how to get the least number of coin changes, but luckily we know how to make  `(11 - ak) + ak = amount`. Hence the problem becomes : `result = min{(current amount -- ak) + 1, current amount}` Now we break the problem into the following:  \(1\) **Last Step**： 最後一枚硬幣肯定為ak \(2\) **Subproblem**：原來問題Ｐ是 “如何用最少硬幣拼出amount \(11\)”,                                 現在可以化成Subproblem為  “如何用最少硬幣拼出amount\(11\) -- ak” 
2. **Transfer Function 轉移方程：** 轉移方程 `f[x] = min {f[x-1] + 1, f[x-2] + 1, f[x-5] + 1}`
3. **Init State and Set Boundaries 初始條件和邊界情況：** \(1\) init state is f\(0\) = 0 \(2\) 初始條件：預設所有的element都為MAX，建一個dp = \[MAX for i in range\(amount + 1\)\] \(3\) 邊界情況：如果 x -- 1, x -- 2, x -- 5 &lt; 0為邊界，意即不能拼出amount\(11\)，那我們就應該要停止計算，跳過它\(continue\)，並且初始化設定f\(x-coin\) 為＋infinity\(max\) \(4\) additional check: 1. 當coins為0時，要return -1    2. 當dp\[amount\(11\)\]為MAX時，要return -1 
4. **Calculate Sequence 計算順序：** 計算從f\(1\), f\(2\), f\(3\), ....f\(11\)，因此for loop的範圍從\(1, amount + 1\) （0個coin不考慮，至少1個coin）

### Full Implementation

```python
    def coinChange(self, coins: List[int], amount: int) -> int:
        
        if not coins or len(coins) == 0:
            return -1
        
        
        # step1. define the state
        # we don't know how to approach the amount, but we know how to make (11 - ak) + ak = amount
        # hence it becomes result = min(11 - ak) + 1
        
        # step2. transfer function: result = (amount - ak) + 1
        # f(11) = min{f(11-1) + 1, f(11-2) + 1, f(11-5) + 1}
        
        # step3. init state and boundaries
        # init state is f(0) = 0
        # if x-1, x-2, x-5 < 0 then we should stop <-- use +MAX
        
        # step4. calc sequence
        # sequence should be f(1), f(2), f(3), ... f(11)
        
        MAX = sys.maxsize
        dp = [MAX for i in range(amount+1)]
        
        
        dp[0] = 0
        for i in range(1,amount+1):
            
            for coin in coins:
                
                if i - coin < 0 or dp[i-coin] == MAX:
#                    dp[i] = MAX  (Note: we already have it set as max during init step) 
                    continue                
                dp[i] = min(dp[i-coin]+1, dp[i])
                
        if dp[amount] == MAX:
            return -1
        
        
        return dp[amount]
```

