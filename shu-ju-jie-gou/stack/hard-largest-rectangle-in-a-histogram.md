# \[Hard\] Largest Rectangle in a Histogram /       \[Hard\] Maximal Rectangle

\*\*\*\*[**Largest Rectangle in Histogram**](https://leetcode.com/problems/largest-rectangle-in-histogram/)  
Given _n_ non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.  
  
For example, `heights = [2,1,5,6,2,3]`Then the largest rectangle area would be showing below:  
`max_rect = 10`

![](https://assets.leetcode.com/uploads/2018/10/12/histogram_area.png)

## Largest Rectangle in Histogram

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

### 2. Stack: O\(n\)/O\(n\)

**思路：Next Smaller Element with Stack。用Stack來記錄目前height最低的left\_index。**  
為什麼要用stack呢？  
At each step we need the information of previously seen "candidate" bars - bars which give us hope. These are the bars of increasing heights. And since they'll need to be put in the order of their occurence, stack should come to your mind.

Using 'Next Smaller Element' Monotone Stack concept, we can find the width of current max rectangles, plus minimum height among all these width indexes.

```python
def largestRectangleArea(self, heights: List[int]) -> int:
    
    stack = []
    max_rect = 0
    heights = [0] + heights + [0]   # 易錯點：前後都要加上[0]，否則等等會out of range
    
    # right_index i
    for i in range(len(heights)):
        # while len(stack) != 0 and 
        # found lower heights[i] than we've seen so far. 
        while stack and heights[i] < heights[stack[-1]]:
            # l_idx is the index of min_height so far.
            l_idx = stack.pop()
            height = heights[l_idx]
            width = i - stack[-1] - 1    # 易錯點：-1是因為在原來heights已加上前後[0]來保持 
                                         # not going out of range
                                         # 易錯點2：是stack[-1]，而非l_idx
            max_rect = max(max_rect, height * width)
        stack.append(i)
        
    return max_rect
```

#### References:

[https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/452612/Thinking-Process-for-Stack-Solution](https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/452612/Thinking-Process-for-Stack-Solution)

這動畫很好地演示了如何找max\_rectangle using stack  
[https://www.youtube.com/watch?v=VNbkzsnllsU](https://www.youtube.com/watch?v=VNbkzsnllsU)

#### Related Qs:

* 1475. Final Prices With a Special Discount in a Shop
* 907. Sum of Subarray Minimus
* 85. Maximum Rectangle
* 402. Remove K Digits
* 456.  132 Pattern
* 1063. Number of Valid Subarrays
* 739. Daily Temperatures
* 1019. Next Greater Node In Linked List

## Maximal Rectangle

### 1. Brute Force: O\($$N^2$$\* M\) / O\(N \* M\)

```python
[["1","0","1","0","0"],
 ["1","0","1","1","1"],
 ["1","1","1","1","1"],
 ["1","0","0","1","0"]]

# 錯誤
# 如果不把dp(or min_heights)清空，就會變成下面這樣。
[1, 0, 1, 0, 0]
[2, 0, 2, 1, 1]
[3, 1, 3, 2, 2]
[4, 1, 3, 3, 2]

# 正確
# dp array  //  max_rect so far 
[1, 0, 1, 0, 0] 1    # iter 1
[2, 0, 2, 1, 1] 3    # iter 2
[3, 1, 3, 2, 2] 6    # iter 3
[4, 0, 0, 3, 0] 6    # iter 4 (last row)
```

```python
import sys

def maximalRectangle(self, matrix: List[List[str]]) -> int:
    
    if not matrix or not matrix[0]:
        return 0
    
    max_rect = 0
    rows = len(matrix)
    cols = len(matrix[0])
    dp = [0] * cols               # To keep track of local max (as curr max heights). 
                                  # By each 'row'.
    
    for i in range(rows):
        for j in range(cols):
            if matrix[i][j] == '1':
                dp[j] += 1
            else: # matrix[i][j] == '0'
                dp[j] = 0         # 這步很重要，如果看到0，就必須要化成0，否則會跟上層答案一樣。
        max_rect = max(max_rect, self.findMax(dp))

    return max_rect 

# same solution as  84. max rect in a histogram
def findMax(self, heights):
    
    curr_max = 0
    
    for start in range(len(heights)):
        min_height = sys.maxsize
        for end in range(start, len(heights)):
            min_height = min(min_height, heights[end])
            width = end - start + 1
            curr_max = max(curr_max, min_height * width)
    
    return curr_max
            
```

### 2. Stack: O\(N\*M\) / O\(M\) 

Time Complexity: O\(N\*M\) where N = \# of rows on matrix, M = \# of cols on each row  
Space Complexity: O\(M\) where M = \# of cols on each row

```python
def maximalRectangle(self, matrix: List[List[str]]) -> int:

    if not matrix or not matrix[0]:
        return 0
        
    max_rect = 0
    rows = len(matrix)
    cols = len(matrix[0])
    curr_max = [0] * cols
    
    for i in range(rows):
        for j in range(cols):
            # or do this
            #curr_max[j] = 0 if matrix[i][j] == '0' else curr_max[j] + 1
            if matrix[i][j] == '0':
                curr_max[j] = 0
            else:
                # here to find local max
                curr_max[j] = curr_max[j] + 1
        # here to find global max
        max_rect = max(max_rect, self.findMax(cur_max))
    return max_rect

# Same way of finding max_rect in histogram. 
def findMax(self, heights):
    
    stack = []
    heights = [0] + heights + [0]
    curr_max = 0
    
    for r_idx in range(len(heights)):
        while stack and heights[r_idx] < heights[stack[-1]]:
            l_idx = stack.pop()
            height = heights[l_idx]
            width = r_idx - stack[-1] - 1
            curr_max = max(curr_max, width * height)
        stack.append(r_idx)
    
    return curr_max
    
```

