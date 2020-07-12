# \[Medium\] Search In a 2D Matrix

[Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)  
Write an efficient algorithm that searches for a value in an _m_ x _n_ matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

## Thought Process

### Binary Search

Search in Sorted Array --&gt; 用Binary Search。   
有幾個眉角要注意：  
\(1\)定義col, row的長度:  `row, col = len(matrix), len(matrix[0])`  
\(2\)總長度\(end\): `end =  row*col -1`  
\(3\)位置上的值：`matrix[i][j] = matrix[position//col][position%col]`   
where position is the index in 1D array. 

{% hint style="info" %}
易錯點：如何找到`Matrix[i][j]`？  
假設我們現在有row, col定義為row = len\(matrix\), col = len\(matrix\[0\]\)，  
我們可以知道，要找**`matrix[i][j] = matrix[position // col][position % col]`**  
即 position 除以 col 的商為 row ，餘數為 col     
  
ex: \[\[1, 3, 5, 7\], \[10, 11, 16, 20\], \[23, 30, 34, 50\]\]   
         col col col col  
row \[  \[ 1 , 3 , ****_**5**_ , 7 \],  &lt;--- 5的位置為`matrix[0][2], 即 2//3 = 0, 2%3 = 2`  
row    \[10,11,16,20\],  
row    \[23,30,34,50\]\]  
{% endhint %}

## Code

{% tabs %}
{% tab title="Python" %}
```python
def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
     
     # handle row is zero
    if not matrix or len(matrix) == 0:
         return False
     # handle col is zero ##
    if not matrix[0] or len(matrix[0]) == 0:
         return False
         
    row = len(matrix)
    col = len(matrix[0])
    #len of entire list is 'row*col' 
    start, end = 0, row*col-1
    
    while start + 1 < end:
         mid = start + (end-start)//2
         #value [i][j] = [mid//col][mid%col]
         value = matrix[mid//col][mid%col]
         if value == target:
              return True
         elif value > target:
              end = mid
         else: #value < target
              start = mid
    
    # found it either at start or end position 
    if matrix[start//col][start%col]==target or matrix[end//col][end%col]==target:
         return True
    # not found
    return False     
```
{% endtab %}
{% endtabs %}

