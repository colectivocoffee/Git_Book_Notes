# \[Medium\] Generate Parentheses

Generate Parentheses

## Thought Process & Code

### 1. Backtracking DFS

Time Complexity: Catalan number $$O(\frac {4^n} {\sqrt{n}} )$$ 

Space Complexity: $$O(\frac {4^n} {\sqrt{n}} )$$ 

> 用Backtracking的方式來遍歷所有的可能性。我們只把valid paranethesis加進結果，  
> 分兩種可能：\(1\) `left > 0` \(2\) `left < right`，並且在遞歸出口時，把目前結果加到結果集\(result\)裡。

```python
def generateParenthesis(self, n: int) -> List[str]:
    result = []
    self.dfs("", result, n, n)
    return result

def dfs(self, curr_res, result, left, right):
    # 遞歸出口
    if left == 0 and right == 0:
        result.append(curr_res)
        return 
    
    if left > 0:
        self.dfs(curr_res + "(", result, left - 1, right)
    if left < right:
        self.dfs(curr_res + ")", result, left, right - 1)
    
```

> 也是用Backtracking的方式來遍歷所有的可能性。分兩種可能：\(1\) left &gt; 0 \(2\) right &gt; 0，並且同時搭配遞歸出口 `left > right` 。最後再把完成的結果添加到result裡面。

```python
def generateParenthesis(self, n: int) -> List[str]:
    result = []
    self.dfs("", result, n, n)
    return result
    
def dfs(self, curr_res, result, left, right):
    # 已完成結果1 or 結果2，遞歸出口，返回
    if left > right:
        return
    # 結果1
    if left > 0:
        self.dfs(curr_res + "(", result, left - 1, right)
    # 結果2
    if right > 0:
        self.dfs(curr_res + ")", result, left, right - 1)
    if left == 0 and right == 0:
        result.append(curr_res)
        return

```

