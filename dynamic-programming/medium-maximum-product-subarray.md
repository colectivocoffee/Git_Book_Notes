# \[Medium\] Maximum Product Subarray

### Question

Given an integer array `nums`, find the contiguous subarray within an array \(containing at least one number\) which has the largest product. [Leetcode \(152\)](https://leetcode.com/problems/maximum-product-subarray/)

### Example 

```text
Example:
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

### Thought Process

1. make sure the state
2. transfer function
3. add init state and define boundaries
4. solve the ordering

1**.確定狀態**  
 \(1\) 先假設只有正數, 那就只要把前面n個數乘起來求Maximum, 即 `result = nums[i-1] * nums[i]`  
 \(2\) 但因為 nums\[i\] 有可能是負數, 因此需要分成兩部份來看: **nums\[ i \] &gt; 0 & nums\[ i \] &lt; 0**  
    \# 當 nums\[ i \] &gt; 0:  
       maxArray\[ i-1\] \* 正數 =&gt; 找到連續乘積最大的max   
       minArray \[ i-1\] \* 正數 =&gt; 乘積最小的min  
    \# 當 nums\[ i \] &lt; 0:   
       maxArray & minArray  
       maxArray\[ i-1\] \* 負數 =&gt; 有機會從Max變成min   
       minArray \[ i-1\] \* 負數 =&gt; 有機會成為Maximum Product

**2.轉移方程：**   

$$
result = \max \left\{ currentP[i-1] * nums[ i ] \right\}
$$

    並且需要考慮當前nums \[ i \] 的值 =&gt;  nums \[ i \] &gt; 0 & nums \[ i \] &lt; 0

**3. 初始狀態和邊界情況：**

  \(1\) nums長度最小為1, 因此需要考慮當前nums \[ i \] 的值   
  \(2\) 邊界最長為nums的長度, 因此可以直接deepCopy nums, 即 `array = nums[::]`    
  \(3\) for loop 從 “1” 開始

**4. 計算順序：**   
   It should start from nums\[1\], nums\[2\], ... to nums\[ `n - 1`\] 

時間複雜度 Ｏ\(n\) ：枚舉了數組的長度  
空間複雜度 Ｏ\(n\) ：消耗了等長的空間

### Full Implementation

```python
def maxProduct(self, nums):
    
    if not nums:
        return -1
        
    max_array = nums[::]
    min_array = nums[::]
    
    for i in range(1, len(nums)):
        
        if nums[i] > 0: 
            max_array[i] = max(max_array[i], max_array[i-1] * nums[i])
            min_array[i] = min(min_array[i], min_array[i-1] * nums[i])
        else: 
            max_array[i] = max(max_array[i], min_array[i-1] * nums[i])
            min_array[i] = min(min_array[i], max_array[i-1] * nums[i])
            
    return max(max_array)   
```



