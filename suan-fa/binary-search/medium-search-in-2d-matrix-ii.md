# \[Medium\] Search In 2D Matrix II

[Search In 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)  
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

* Integers in each row are sorted in ascending from left to right.
* Integers in each column are sorted in ascending from top to bottom.

#### Example

```text
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = `5`, return `true`.  
Given target = `20`, return `false`.

{% hint style="info" %}
**Idea**: Move Col/Row pointers from Top-Right/Bottom-Left corner.
{% endhint %}

## Thought Process

這題乍看之下，會覺得可以用Binary Search，但仔細看就會發現，如果把這個2D matrix轉化為1D list的時候，並不是有序的\(either sorted or rotated sorted\)。因此，要用Binary Search的話，前提條件就是是rotated sorted/sorted array，所以如果硬要用Binary Search實現的話就會比較複雜。  
  
由下面的矩陣，我們可以發現到從top-right往左看，都是sorted；往下看，也都是sorted。因此，我們可以定義兩個指針row&col，並且從top-right移動他們。  
**`row:`** `0 --> len(matrix)-1`  
**`col:`**`len(matrix[0])-1 --> 0`

```text
[            <-------- @[15]   
  [1,   4,  7, 11, 15], |
  [2,   5,  8, 12, 19], |
  [3,   6,  9, 16, 22], |
  [10, 13, 14, 17, 24], V
  [18, 21, 23, 26, 30]
]
```

Time Complexity: O\(m+n\) or O\(row+col\)

## Code

{% tabs %}
{% tab title="Python" %}
```python
# row, col move from top-right/bottom-left corner
def searchMatrix(self, matrix, target):

    if not matrix or len(matrix) == 0:
        return False
    if not matrix[0] or len(matrix[0]) == 0:
        return False
    
    row = 0
    col = len(matrix[0])-1
    
    while row <= len(matrix)-1 and col >= 0:
        
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] > target:
            col -= 1
        elif matrix[row][col] < target:
            row += 1
    
    # not found
    return False
        
```
{% endtab %}
{% endtabs %}

