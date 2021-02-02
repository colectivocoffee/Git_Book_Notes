# \[Easy\] Pascal's Triangle / \[Medium\] Pascal's Triangle II

## [\[Easy\] Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/) \(2169/128\)

Given a non-negative integer _numRows_, generate the first _numRows_ of Pascal's triangle.

```text
Example
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

### 1. DP: O\(numRows^2\) / O\(numRows^2\)

Time Complexity: O\(numRows^2\)  
Although updating each value of `triangle` happens in constant time, it is performed O\(numRows^2\) times. To see why, consider how many overall loop iterations there are. The outer loop obviously runs numRowsnumRows times, but for each iteration of the outer loop, the inner loop runs rowNumrowNum times. Therefore, the overall number of `triangle` updates that occur is $$1 + 2 + 3 + \ldots + numRows1+2+3+…+numRows$$. which, according to Gauss' formula, can be $$\dfrac {numRows (numRows+1)} {2} ​$$

Space Complexity: O\(numRows^2\)  
Because we need to store each number that we update in `triangle`, the space requirement is the same as the time complexity. 

```python
#
#    [1,1]
#     \ / 
#   [1,2,1]
#
# row   curr_row    prev_row   item
# 3       [1]       [1, 1]        1
# 4       [1]       [1, 2, 1]     1
# 4      [1, 3]     [1, 2, 1]     2
# 5       [1]       [1, 3, 3, 1]  1
# 5      [1, 4]     [1, 3, 3, 1]  2
# 5     [1, 4, 6]   [1, 3, 3, 1]  3
def generate(self, numRows: int) -> List[List[int]]:

    result = [[1]]

    for row in range(1, numRows):
        # row1 & row2 已經先建好，這樣才能取prev[item-1] + prev[item]
        # 來完成 1+1 = 2 的計算
        curr = 1
        curr_row = [curr]
        prev_row = result[row-1]

        for item in range(1, row): 
#           print(curr_row, prev_row, item)
            curr_row.append(prev_row[item-1] + prev_row[item])
        
        curr_row.append(1)
        result.append(curr_row)
    
    return result   
```

或者是用下面公式 `curr = int(curr * (row - item) / item)`

```python
def generate(self, numRows: int) -> List[List[int]]:
    
    result = []
    for row in range(1, numRows+1):
        curr = 1
        curr_row = []
        for item in range(1, row+1):
            curr_row.append(curr)
            curr = int(curr * (row - item) / item)
        result.append(curr_row)
    return result
```

## [\[Medium\] Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/) \(1217/217\)

Given an integer `rowIndex`, return the `rowIndexth` 's row of Pascal's triangle.  
Notice that the row index starts from **0**.

### 1. DP: O\(rowIndex^2\) / O\(rowIndex^2\)

```python
def getRow(self, rowIndex: int) -> List[int]:

    result = []
    for row in range(1, rowIndex+2):
        curr = 1
        curr_row = []

        for item in range(1, row+1):
            curr_row.append(curr)
            curr = int(curr * (row - item) / item)
        result.append(curr_row)                

    return result[rowIndex]        
```

**Follow up:** Could you optimize your algorithm to use only _O_\(_k_\) extra space?

> 思路：去除result, 直接返回curr\_row

```python
def getRow(self, rowIndex: int) -> List[int]:

    for row in range(1, rowIndex+2):
        curr = 1
        curr_row = []

        for item in range(1, row+1):
            curr_row.append(curr)
            curr = int(curr * (row - item) / item)

        if len(curr_row) == rowIndex+1: 
            return curr_row                
```

