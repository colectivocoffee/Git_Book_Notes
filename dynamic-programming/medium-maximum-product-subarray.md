# \[Medium\] Maximum Product Subarray

## [\[Medium\] Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)      \(6210/205\)

Given an integer array `nums`, find the contiguous subarray within an array \(containing at least one number\) which has the largest product. [Leetcode \(152\)](https://leetcode.com/problems/maximum-product-subarray/)

```text
Example:
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

> 思路：  
> `max_prod_subarray`可以藉由下面兩種方式完成。  
> \(1\) all positive numbers  
> \(2\) paired/even negative numbers  
> 除此之外，如果遇到其他數字如zeros, odd negative numbers都會讓max product\(最大乘積\)變小。  
> 因此，我們在處理數字num時需要特別注意上面這幾種情況。  
>   
> 要如何確保不會碰到zeros, odd negative numbers呢？  
> 我們可以藉由同時紀錄 `curr_max` & `curr_min` 來保證如果遇到odd negative numbers時，可以在下一個negative number出現時，把它給中和掉。（Sequence DP 解法）

### 1. Two Pointer Brute Force: O\(N^2\) / O\(1\)

```python
def maxProduct(self, nums: List[int]) -> int:

    if not nums:
        return 0
    
    # to solve edge case like nums = [-3]
    global_max = nums[0]

    for left in range(len(nums)):
        # start accumulating from left pointer 
        accu = 1
        for right in range(left, len(nums)):
            accu = accu * nums[right]
            global_max = max(global_max, accu)

    return global_max
```

### 2. Sequence DP: O\(N\) / O\(N\)

時間複雜度 Ｏ\(n\) ：枚舉了數組的長度  
空間複雜度 Ｏ\(n\) ：消耗了等長的空間

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

**Sequence DP Intuition**

Rather than looking for every possible subarray to get the largest product, we can scan the array and solve smaller subproblems.

Let's see this problem as a problem of getting the highest combo chain. The way combo chains work is that they build on top of the previous combo chains that you have acquired. The simplest case is when the numbers in `nums` are all positive numbers. In that case, you would only need to keep on multiplying the accumulated result to get a bigger and bigger combo chain as you progress.

However, two things can disrupt your combo chain:

* Zeros
* Negative numbers

**Zeros** will reset your combo chain. A high score which you have achieved will be recorded in placeholder `result`. You will have to _restart_ your combo chain after zero. If you encounter another combo chain which is higher than the recorded high score in `result`, you just need to update the `result`.

**Negative numbers** are a little bit tricky. A single negative number can flip the largest combo chain to a very small number. This may sound like your combo chain has been completely disrupted but if you encounter another negative number, your combo chain can be saved. Unlike zero, you still have a hope of saving your combo chain as long as you have another negative number in `nums` \(Think of this second negative number as an antidote for the poison that you just consumed\). However, if you encounter a zero while you are looking your another negative number to save your combo chain, you lose the hope of saving that combo chain.

While going through numbers in `nums`, we will have to keep track of the maximum product up to that number \(we will call `max_so_far`\) and minimum product up to that number \(we will call `min_so_far`\). The reason behind keeping track of `max_so_far` is to keep track of the accumulated product of positive numbers. The reason behind keeping track of `min_so_far` is to properly handle negative numbers.

`max_so_far` is updated by taking the **maximum** value among:

1. Current number.
   * This value will be picked if the accumulated product has been really bad \(even compared to the current number\). This can happen when the current number has a preceding zero \(e.g. `[0,4]`\) or is preceded by a single negative number \(e.g. `[-3,5]`\).
2. Product of last `max_so_far` and current number.
   * This value will be picked if the accumulated product has been steadily increasing \(all positive numbers\).
3. Product of last `min_so_far` and current number.
   * This value will be picked if the current number is a negative number and the combo chain has been disrupted by a single negative number before \(In a sense, this value is like an antidote to an already poisoned combo chain\).

`min_so_far` is updated in using the same three numbers except that we are taking **minimum** among the above three numbers.

In the animation below, you will observe a negative number `-5` disrupting a combo chain but that combo chain is later saved by another negative number `-4`. The only reason this can be saved is because of `min_so_far`. You will also observe a zero disrupting a combo chain.



