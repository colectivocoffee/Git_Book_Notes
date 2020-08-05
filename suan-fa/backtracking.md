# Backtracking

## Backtracking（回溯法）

Backtracking（回溯法）常用於traverse列表內的所有Subsets。通常這種題目都會跟Combination/Permutation有關，當然還有典型的N皇后問題。

回溯法（backtrack）常用于遍历列表所有子集，是 DFS 深度搜索一种，一般用于全排列，穷尽所有可能，遍历的过程实际上是一个决策树的遍历过程。时间复杂度一般 O\(N!\)，它不像动态规划存在重叠子问题可以优化，回溯算法就是纯暴力穷举，复杂度一般都很高。

模板：

```python
result = []
while backtrac(選擇列表,
```

### 1. Subsets

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

