# Dynamic Programming

## 什麼是Dynamic Programming \(動態規劃\)？

所謂的動態規劃 DP，就是利用**memoize數組**來記錄已計算過的結果。但要如何知道什麼時候要記錄呢？  
此時我們發現，當一個題目可以**“把大問題化成小問題"**，小問題化成小小問題，以此類推，利用重複計算的特性，就可以用動態規劃來解決。

### DP核心

動態規劃問題的一般形式就是求最xx值\(e.g. Maximum Increasing Subsequence, Min Edit Distance, Min Coin Change, Min Cost to Paint House, ....\)。

> **求解DP的方法就是窮舉，列出所有可能性，並且記錄當下的最xx值到dp table。**

因為要求最xx值，必須要把所有可行的答案都窮舉出來，並且在其中找最xx值。然而，DP類型的問題都存在”**重疊子問題\(duplicate subproblems\)“**，如果只是暴力解會造成Time Limit Exceeded\(TLE\)，因此需要用`memo` or `DP table` 等技巧優化窮舉過程。

DP問題會有**”最優子結構\(min/max answer of a subproblem\)“**，透過子問題的最xx值來得到原始問題的最xx值。

還有另外一個問題，要窮舉出所有可行解並不容易，只有列出**正確的”狀態轉移方程\(transfer function\)"`e.g f[i] = min/max{f[i-1]+item, f[i-1]}`**才能窮舉出所有結果。其中最難的是寫出transfer function公式。

#### 如何想出狀態轉移方程？

> 確定"狀態" -&gt; 定義DP函數的含義 -&gt; 明確"選擇" -&gt; 明確Base Case

* 確定狀態
* 定義DP函數
* 確定選擇
* 定義Base Case

列出動態轉移方程，就是在解決”如何窮舉“的問題。然而很多的窮舉需要用Recursion實現，或是解空間subproblems很複雜，不容易窮舉完整。

### DP's Time Complexity:

> Time Complexity = $$(num\ of\ subproblems) * (time\ to\ solve\ each\ subproblem) $$ 
>
> 時間複雜度 = subproblem總數 \* 解決每個subproblem所需時間

### DP 求解方式

DP求解方式分兩種：  
\(1\) 從小到大\(Bottom-Up\)，從小到大遞推。  
\(2\) 從大到小\(Top-Down\)，從大到小記憶化搜索。  
  
DP，自底向上。當我們不知道如何求解時，看**最後一步**，一步一步從底向上推。

DP方法自下而上Bottom-Up: f\[0\], f\[1\], ..., f\[N\]  
記憶化方法自上而下Top-Down:  f\(N\), f\(N-1\), ...

{% hint style="info" %}
貪心算法常靠背答案，因此如果能用DP的題目來解，就儘量用DP。
{% endhint %}

More Info [Here](https://leetcode.com/problems/house-robber/discuss/156523/From-good-to-great.-How-to-approach-most-of-DP-problems.)

## DFS 和 DP 的區別

對於DFS的問題，通常都是subproblem沒有交集，所以大部分用Recursion或Binary Search解決。ex: Binary Tree's Subproblem  
然而，對於DP的問題，**subproblem有交集**，可以從大問題化成和大問題有交集的小問題而解決。

## DP 類型

### \(1\) Sequence DP -- 40%

思考流程：當思考動DP最後一步`f[i]`時，這一步的選擇依賴於上一步的某種狀態`f[i-1]`。  
需要紀錄：序列 + 狀態`f[i]`，或是說狀態下的所有可能性。  
`f[i] = max(f[i-1], f[i-1] + xxxx)`   
tmp holder

* Climbing Stairs \(Easy\)
* Jump Game \(Medium\)
* Longest Increasing Subsequence \(Medium\)
* Decode Ways \(Medium\)
* House Robber \(Easy\)
* House Robber II \(Medium\)
* Word Break \(Medium\)
* Palindrome Partitioning II \(Hard\)
* Best Time To Buy and Sell Stock \(Easy\)
* Best Time To Buy and Sell Stock II \(Easy\)
* Best Time To Buy and Sell Stock III \(Hard\)

### \(2\) Matrix DP -- 10%

* Unique Paths \(Medium\)
* Maximum Product Subarray \(Medium\)
* Unique Paths II
* Minimum Paths Sum

### \(3\) Two Sequence DP -- 40%

* Longest Common Subsequence \(Medium\)
* Edit Distance \(Hard\)

### \(4\) Coin & Backpack DP -- 10%

* Coin Change \(Medium\)
* 01 Backpack \(Medium\)

## DP 解題步驟

1. make sure the state
2. transfer function
3. add init state and define boundaries
4. solve the ordering

## 題目

* [ ] Best Time to Buy and Sell Stock
* [ ] Edit Distance
* [ ] Decode Ways
* [ ] Coin Change
* [ ] Maximum Subarray
* [ ] Maximum Product Subarray
* [ ] Word Break
* [ ] Longest Common Subsequence
* [ ] Longest Increasing Subsequence
* [ ] Palindrome Partitioning II
* [ ] House Robber
* [ ] House Robber II
* [ ] Jump Game
* [ ] Climbing Stairs



