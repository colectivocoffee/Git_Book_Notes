# \[Easy\] Best Time to Buy and Sell Stock

## \[Easy\] [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)         \(7600/338\)

**\(NOTE: 這是只能買賣一次的版本\)**  
Say you have an array for which the _i_th element is the price of a given stock on day _i_.

If you were only permitted to complete at most one transaction \(i.e., buy one and sell one share of the stock\), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

## Thought Process

### \(1\) Brute Force + Memoization: O\(N^2\) / O\(N\)

使用兩個變量，`i & j`來代表買入日和賣出日，而`prices[i]`, `prices[j]`代表買入價格和賣出價格。  
\(1\) `i`代表買入日，`j`代表賣出日  
\(2\) 如果profit比現在的高，則替換新的max。  
\(3\) 注意在第二個j for loop的時候，j需要從i+1開始賣。  
Time Complexity: O\( $$n^2$$ \)   
Space Complexity: O\( $$n$$ \)

#### 如何優化？ 對於memoization數組f，其實並不需要紀錄所有的buy&sell stock值。可以以一個variable maxProfit取代，使得space complexity為O\(1\)。

### \(2\) Greedy: O\(N\) / O\(1\)

看到題目，發現因為只能買賣一次，因此先求全局最小\(buy-in\)，再求全局最大\(profit\)，最後最大 -- 最小即是max\_profit。  
current\_lowest = float\('inf'\)  
max\_profit = 0  
  
Time Complexity:   O\( $$n$$ \)  
Space Complexity: O\( $$1$$ \)

{% hint style="info" %}
NOTE: 當要求全局最小值時，**`float('inf')`**$$+\infty$$ 很好用。
{% endhint %}

## Full Implementation

#### \(1\) Brute Force + Memoization

Because of O\( $$n^2$$ \),  this approach will result in TLE \(Time Limit Exceeded\)

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

#### \(2\) Greedy Search: O\(N\) / O\(1\)

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
        max_profit = max(max_profit, profit)
        
    return max_profit
    
```
{% endtab %}
{% endtabs %}

