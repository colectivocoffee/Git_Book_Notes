# \[Medium\] Find K Closest Elements

[Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/)  
Given a sorted array `arr`, two integers `k` and `x`, find the `k` closest elements to `x` in the array. The result should also be sorted in ascending order. If there is a tie, the smaller elements are always preferred.

#### Example

```text
Input: arr = [1,2,3,4,5], k = 4, x = -1
Output: [1,2,3,4]
```

## Thought Process

由題意可知，x是target值，k是在arr內取k個靠近target的數。靠近target的數代表的是可以看距離。

### 1. Two Pointers \(-&gt; / &lt;-\): O\(n\)/O\(1\)

我們可以利用一個variable `remove_length` 來動態地確認還需要多少個數，當remove\_length == 0時即達到題目的k個數要求便結束。Left, Right 為在arr左右兩邊的兩個pointers並且向中間移動。當remove\_length == 0時，left&right代表的是arr的start&top position。

另外，`length = abs(arr[current_index]-x)`來決定，並且比較左右兩邊，看哪一個跟x target比距離遠。因此可以寫成以下的if statement：

```python
 arr = [1,2,3,4,5], x target = 3, k numbers = 4
  (left)^   x   ^(right)
    (left)^ x   ^(right)

if abs(arr[left]-x) > abs(arr[right]-x):
    left += 1
    remove_length -= 1
    
```

**Time Complexity: O\(n\) arr的長度  
Space Complexity: O\(1\) left, right, remove\_length變量**

### 2. Binary Search: O\(logn\)/O\(1\)

## Code

#### 1. Two Pointers: O\(n\)/O\(1\)

```python
 def findClosestElements(self, arr: List[int], k: int, x: int) -> List[int]:
 
    if not arr or len(arr) == 0:
          return []
    
    # left and right both move from the opposite side
    # remove_length gives you number of elements remaining need to be removed
    left, right = 0, len(arr)-1
    remove_length = len(arr)-k
    
    while remove_length > 0:
          
          if abs(arr[left]-x) > abs(arr[right]-x):
                left += 1
                remove_length -= 1
          else:
                right -= 1
                remove_length -= 1
    
    #[start:stop]
    return arr[left:right+1]
```

