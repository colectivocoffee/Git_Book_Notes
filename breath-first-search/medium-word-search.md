# \[Medium\] Word Search

[Word Search](https://leetcode.com/problems/word-search/)  
Given a 2D board and a word, find if the word exists in the grid.  
The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.  
  
**Example**

```text
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

## Code

### 1. DFS Recursive + visited \(x,y\) array

> 思路：從board找字的話，用DFS上下左右四個方向搜索匹配的word。  
> 但下列幾點要注意：  
> \(1\) **board的邊界**  
> \(2\) **current cell是否已被遍歷過** \(ex: 紀錄visited = Tue並且用dfs recursion探索後，再返回並visited = False\)  
> \(3\) **dfs遞歸出口是 wordId == len\(word\)**

此方法是用額外的數組`visited[][]`。

```python
def exist(self, board: List[List[str]], word: str) -> bool:
    if board == None or len(board[0]) == 0:
        return False
    
    rows = len(board)
    cols = len(board[0])
    
    visited = [[False] * cols for _ in range(rows)]
    # 對每一cell (i,j)都遍歷，
    for i in range(rows):
        for j in range(cols):
            if self.dfs(board, word, i, j, 0, visited):
                return True
    return False
            
def dfs(self, board, word, x, y, wordId, visited):
    if wordId == len(word):
        return True
    # go out of boundary
    if x < 0 or x >= len(board) or y < 0 or y >= len(board[0]) \
        or visited[x][y] \
        or board[x][y] != word[wordId]:    
        return False
    
    visited[x][y] = True
    result = self.dfs(board, word, x + 1, y, wordId + 1, visited) \
            or self.dfs(board, word, x, y + 1, wordId + 1, visited) \
            or self.dfs(board, word, x - 1, y, wordId + 1, visited) \
            or self.dfs(board, word, x, y - 1, wordId + 1, visited)
    visited[x][y] = False
    
    return result 
```

**To Optimize Space Complexity: 簡化visited，把原來的cell on board改成 '\#'，改完後再換回原來的字母。**   


  


