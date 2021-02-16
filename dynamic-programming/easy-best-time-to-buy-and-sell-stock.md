# \[Easy\] Best Time to Buy and Sell Stock / \[Easy\] Stock II / \[Hard\] Stock III

## 股票題型思路

紀錄每天curr結束時的`global_max_profit`。  
計算時，在`curr`天之前\(`prev`\)枚舉所有可能的結果\(`hold / not hold`\)並且記錄下來。  
在最後一天賣出股票or確定最後一天手上沒有股票，所得到的`global_max_profit`為答案。

### 4. 最後一步：

第`curr`天賣股票，第`prev`天買股票`(prev < curr)`時，需要知道第`prev`天之前是不是已經買了股票。

### **1&2&3. 一維＋確定狀態：**

紀錄前N天買賣股票的最大獲利\(`global_max_profit`\)，**並且**在最後第N天已賣股票。

轉移方程：  
`latest_local_profit/f[i-1] = prev_hold + prices[i]  
global_max_profit/curr_not_hold/f[i] = max(global_max_profit, latest_local_profit)`

## \[Easy\] [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)         \(7600/338\)

**\(NOTE: 這是只能買賣一次的版本\)**  
Say you have an array for which the _i_th element is the price of a given stock on day _i_.

If you were only permitted to complete at most one transaction \(i.e., buy one and sell one share of the stock\), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

### \(1\) Brute Force + Memoization: O\(N^2\) / O\(N\)

使用兩個變量，`i & j`來代表買入日和賣出日，而`prices[i]`, `prices[j]`代表買入價格和賣出價格。  
\(1\) `i`代表買入日，`j`代表賣出日  
\(2\) 如果profit比現在的高，則替換新的max。  
\(3\) 注意在第二個j for loop的時候，j需要從i+1開始賣。  
Time Complexity: O\( $$n^2$$ \)   
Space Complexity: O\( $$n$$ \)

Because of O\( $$n^2$$ \),  this approach will result in TLE \(Time Limit Exceeded\)

#### 如何優化？ 對於memoization數組f，其實並不需要紀錄所有的buy&sell stock值。可以以一個variable maxProfit取代，使得space complexity為O\(1\)。

{% tabs %}
{% tab title="Python" %}
```python
def maxProfit(self, prices: List[int]) -> int:
    
    days = len(prices)
    
    if not prices or days == 0 or days == 1:
        return 0
        
    f = [0 for i in range(days)]
    
    for i in range(days):
        for j in range(i+1, days): #i+1 represents it can only sell stock the next day 
            profit = prices[j] - prices[i]
            if profit > f[j]:
                f[j] = profit
    
    return max(f)
```
{% endtab %}
{% endtabs %}

### \(2\) Greedy: O\(N\) / O\(1\)

看到題目，發現因為只能買賣一次，因此先求全局最小\(buy-in\)，再求全局最大\(profit\)，最後最大 -- 最小即是max\_profit。  
current\_lowest = float\('inf'\)  
max\_profit = 0  
  
Time Complexity:   O\( $$n$$ \)  
Space Complexity: O\( $$1$$ \)

{% hint style="info" %}
NOTE: 當要求全局最小值時，**`float('inf')`**$$+\infty$$ 很好用。
{% endhint %}

{% tabs %}
{% tab title="Python" %}
```python
def maxProfit(self, prices: List[int]) -> int:
    
    if not prices or len(prices) <=1:
        return 0
    
    #float('inf') represnts positive infinity, useful whenn finding smallest value
    current_lowest = float('infinity') 
    max_profit = 0
    
    for price in prices:
        current_lowest = min(current_lowest, price)
        profit = price - current_lowest
        #                   max(前一天已賣出, 前一天沒賣 + 今天價格)
        # max_profit/f[i] = max(prev_profit/f[i-1], prev_profit/f[i-1] + prices[i])
        max_profit = max(max_profit, profit)
        
    return max_profit
    
```
{% endtab %}
{% endtabs %}

## [\[Easy\] Best Time To Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)    \(3661/1918\)

Say you have an array `prices` for which the _i_th element is the price of a given stock on day _i_.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like \(i.e., buy one and sell one share of the stock multiple times\).

**Note:** You may not engage in multiple transactions at the same time \(i.e., you must sell the stock before you buy again\). \(可以當日同時買入賣出，只是只有一股\)

### 1. Greedy: O\(N\) / O\(1\)

只要今天比昨天高，就賣出；反之今天比昨天低，就買入，用貪心算法。

we can simply go on crawling over the slope and keep on adding the profit obtained from every consecutive transaction.  
  
From the below graph, we can observe that the sum A+B+C is equal to the difference D corresponding to the difference between the heights of the consecutive peak and valley.

![](../.gitbook/assets/image%20%2816%29.png)

```python
def maxProfit(self, prices: List[int]) -> int:

    result = 0
    for i in range(1,len(prices)):

        if prices[i] > prices[i-1]:
            result += prices[i] - prices[i-1]
    return result
```

### 2. Sequence DP: O\(N\) / O\(N\)

> 思路：買低賣高。  
> 只要看到今日\(`price[i]`\)比昨日\(`price[i-1]`\)高，就賣掉手中的股票；反之，如果今日比昨日低，則買入股票。同時**`紀錄前一日買or不買`**的兩個各自最大profit狀態，沿用到今日，然後再做一遍同樣的流程，直到最後一天。  
>   
> profit的計算方法為:  
> **`curr_not_hold_balance  = prev_hold_balance + today's stock price  
>     curr_hold_balance  = prev_not_hold_balance - today's stock price`**  
> 我們藉由`curr_hold`和 `curr_not_hold`來紀錄目前為止的`max_profit`。  
>   
> 為什麼是DP呢？因為我們一直紀錄到目前day i 為止的`global_max_profit`，直到最後一天，`global_`_`max`_就是全局`local_max_profit`的總和。

同樣的DP思維，可以借用[Jump Game](https://app.gitbook.com/@iscolectivo/s/algonote/dynamic-programming/medium-jump-game)的解法來看。  
  
先問自己，要如何得到`global_max_profit`？  
1\) 如果要有profit，我們必須得確定能賺到`大於0的 global_max_profit`。`(Base Case)`   
2\) Question: 那我們要如何有`global_max_profit`呢？必須要在**最後一天**的到來時，手中沒持有股票並且得到`global_max_profit`。我們需要在每一次/天都能買低賣高，只要看到今天比昨天低，那就買入；反之今天比昨天高，那就賣出；並且同時紀錄**`(1)如果賣出cur_not_hold`** / **`(2)如果不賣出cur_hold`**這兩種狀態，同時和 **`(1)前一天賣出prev_not_hold`** / **`(2)前一天不賣出prev_hold`**比較。這樣我們可以確保在隔天的上漲都能計算到目前的`local_max_profit`，如圖下綠色區塊。  
那我們的問題就變成，要如何在每個區間都能得到`local_max_profit`呢？我們就要讓當下波段最高點的到來時，手中的股票已賣出並且得到`local_max_profit`。我們需要當下波段都買低賣高，並且同時紀錄**`(1)如果賣出`**/ **`(2)如果不賣出`**這兩種狀態。依次進行下去比較...  
3\) 如此重複，直到最後一天為止，最後就可以從`local_max_profit_1 + local_max_profit_2 + ...`得到`global_max_profit`答案。  


![](../.gitbook/assets/image%20%2815%29.png)

```python
def maxProfit(self, prices: List[int]) -> int:

    cur_hold, cur_not_hold = -float('inf'), 0

    for price in prices:

        prev_hold, prev_not_hold = cur_hold, cur_not_hold

        cur_hold = max(prev_hold, prev_not_hold - price)
        cur_not_hold = max(prev_not_hold, prev_hold + price)


    return cur_not_hold if prices else 0
```

## Best Time To Buy and Sell Stock III

### 一維＋狀態

紀錄前N天買賣股票的最大獲利，並且在第N-1天:   
1. 未買賣股票  
2. 買了第一次股票還沒賣  
3. ...  
5. 已經第二次賣了股票 

