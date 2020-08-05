# Backtracking

## Backtracking（回溯法）

Backtracking（回溯法）常用於traverse列表內的所有Subsets。通常這種題目都會跟Combination/Permutation有關，當然還有典型的N皇后問題。

回溯法（backtrack）常用于遍历列表所有子集，是 DFS 深度搜索一种，一般用于全排列，穷尽所有可能，遍历的过程实际上是一个决策树的遍历过程。时间复杂度一般 O\(N!\)，它不像动态规划存在重叠子问题可以优化，回溯算法就是纯暴力穷举，复杂度一般都很高。

模板：

```python
result = []
while backtrack(選擇列表,路徑):
    if 滿足結束條件:
        result.add(路徑）
        return
    for 選擇 in 選擇列表:
        做選擇
        backtrack(選擇列表,路徑)
        撤銷選擇
```

### 1. Subsets

A huge number of coding interview problems involve dealing with Permutations and Combinations of a given set of elements. The pattern Subsets describes an efficient Breadth First Search \(BFS\) approach to handle all these problems.

![](.gitbook/assets/subsets.jpg)



How to identify the Subsets pattern:

* Problems where you need to find the combinations or permutations of a given set
* Problems featuring Subsets pattern:
* Subsets With Duplicates \(easy\)
* String Permutations by changing case \(medium\)

此題存在4種解法。  
\(1\) Backtracking/DFS \( +Recursion \)  
\(2\) BFS



#### 題目

* [ ] word search
* [ ] Subsets I & II
* [ ] Permutations I & II
* [ ] Letter Case Permutation
* [ ] Combination
* [ ] Combination Sum I & II & III
* [ ] Generate Parentheses
* [ ] Palindrome Partitioning
* [ ] Letter Combinations of a Phone Number
* [ ] Sudoku Solver
* [ ] N-Queens

