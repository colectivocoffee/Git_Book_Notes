# \[Hard\] Largest Rectangle in a Histogram

## Code

### 1. Brute Force: O\(n^2\)/O\(n\) possibly TLE

思路：枚舉法。使用兩個pointers start&end，start為left pointer，end為right pointer。

left pointer從index=0開始出發，right pointer則是從left pointer開始走。而largest rectangle的算法，是由`min_height`定義高度，乘以寬度`end-start+1`即可。

```python
def largestRectangleArea(self, heights: List[int]) -> int:
    
    max_rect = 0
    
    for start in range(len(heights)):
        min_height = sys.maxsize
        for end in range(start, len(heights)):
            min_height = min(min_height, heights[end])
            max_rect = max(max_rect, min_height * (end - start + 1))
            
    return max_rect
```

### 2. Stack: 

