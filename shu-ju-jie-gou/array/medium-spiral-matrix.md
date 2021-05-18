# \[Medium\] Spiral Matrix

## [\[Medium\] Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)    \(3870/671\)

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

