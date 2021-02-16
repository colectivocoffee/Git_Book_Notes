# Dynamic Programming

## 什麼是Dynamic Programming \(動態規劃\)？

所謂的動態規劃 DP，就是利用**memoize數組**來記錄已計算過的結果。但要如何知道什麼時候要記錄呢？  
此時我們發現，當一個題目可以**“把大問題化成小問題"**，小問題化成小小問題，以此類推，利用重複計算的特性，就可以用動態規劃來解決。

DP求解方式分兩種：  
\(1\) 從小到大\(Bottom-Up\)，從小到大遞推。  
\(2\) 從大到小\(Top-Down\)，從大到小記憶化搜索。  
  
DP，自底向上。當我們不知道如何求解時，看**最後一步**，一步一步從底向上推。

DP方法自下而上Bottom-Up: f\[0\], f\[1\], ..., f\[N\]  
記憶化方法自上而下Top-Down:  f\(N\), f\(N-1\), ...  
  
More Info [Here](https://leetcode.com/problems/house-robber/discuss/156523/From-good-to-great.-How-to-approach-most-of-DP-problems.)  
  
Sometimes you do not need to store the whole DP table in memory, the last two values or the last two rows of the matrix will suffice.

{% hint style="info" %}
貪心算法常靠背答案，因此如果能用DP的題目來解，就儘量用DP。
{% endhint %}

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



