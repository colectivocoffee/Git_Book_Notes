# \[Medium\] Spiral Matrix

## [\[Medium\] Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)    \(3870/671\)

Given an `m x n` `matrix`, return _all elements of the_ `matrix` _in spiral order_.

![](../../.gitbook/assets/image%20%28106%29.png)

```text
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
```

### 1. 用r, c, idx pointer來更新方向 + dp table:    O\(N\) / O\(N\)

> 關鍵點：  
> 1. 用pointer定義方向：每到邊界時，就要換個方向走。而這個方向是 right -&gt; down -&gt; left -&gt; up。  
> 2. 用dp table紀錄軌跡：同時我們需要一個dp n\*n table去紀錄這個點是否已看過，來確保這個環會越來越往中心走。  
> 3. 用idx = \(idx + 1\) % 4 來移動pointer

```python
def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
    
    result = []
    if not matrix:
        return result
    
    rows = len(matrix)
    cols = len(matrix[0])
    # pointer
    r, c, idx = 0, 0, 0
    #  ^ -----> 
    #  |       |
    #  |       |
    #  <-----  v
    # traversal direction
    # clockwise: right -> down -> left -> up
    #          (1,0)->(0,1)->(-1,0)->(0,-1)
    direction_c = [1, 0, -1, 0]
    direction_r = [0, 1, 0, -1]
    
    dp = [[False]*cols for _ in range(rows)]
    
    for _ in range(rows * cols):
        result.append(matrix[r][c])
        dp[r][c] = True
        next_r = r + direction_r[idx] 
        next_c = c + direction_c[idx]
        
        # within the boundary, no need to change direction
        if 0 <= next_r < rows and 0 <= next_c < cols and not dp[next_r][next_c]: 
            r = next_r
            c = next_c
        else:
            # move direction clockwise
            idx = (idx + 1) % 4
            r = r + direction_r[idx]
            c = c + direction_c[idx]
    
    return result
```

### 2. Layer by Layer:    O\(N\) / O\(1\)

![](../../.gitbook/assets/image%20%28105%29.png)

```python
def spiralOrder(self, matrix):
    def spiral_coords(r1, c1, r2, c2):
        for c in range(c1, c2 + 1):
            yield r1, c
        for r in range(r1 + 1, r2 + 1):
            yield r, c2
        if r1 < r2 and c1 < c2:
            for c in range(c2 - 1, c1, -1):
                yield r2, c
            for r in range(r2, r1, -1):
                yield r, c1

    if not matrix: return []
    ans = []
    r1, r2 = 0, len(matrix) - 1
    c1, c2 = 0, len(matrix[0]) - 1
    while r1 <= r2 and c1 <= c2:
        for r, c in spiral_coords(r1, c1, r2, c2):
            ans.append(matrix[r][c])
        r1 += 1; r2 -= 1
        c1 += 1; c2 -= 1
    return ans
```

