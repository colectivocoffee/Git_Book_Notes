# \[Medium\] Product of Array Except Self

[Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)  
Given an array `nums` of _n_ integers where _n_ &gt; 1, return an array `output` such that `output[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

## Thought Process

分成三步驟。一，從i把這個list劈成兩半，左半和右半。二，計算左半每個的product，再計算右半每個的product。三，最後把左半i和右半i乘起來，即是答案 \(因為不包含自己\)。  
  
Subproblem:   
firstHalf transfer function: `first[i] = first[i-1] + nums[i-1]`  
secondHalf transfer function: `second[i] = second[i+1] + nums[i+1]`

iterate from start to end很好寫，但如果要寫from end to start，就要注意boundaries和for loop寫法。此題from end to start 寫法有三種index + 一種element：

```python
nums = [ 1, 2, 3, 4]
         ^  ^  ^
     -1  0  1  len(nums)-2
#1. start, stop, step. Start from len(nums)-2
for i in ranage(len(nums)-2, -1, -1)

#2. range(start, stop). stop at len(nums)-1 to skip last one.
for i in reversed(range(0,len(nums)-1))

#3. use [::-1] to reverse traversal
for i in range(0,len(nums)-1)[::-1]

#4. list[start:stop:step]. But this iterate thru element 
for element in nums[len(nums)-1:-1:-1]
```

## Code

### 1. Expand From Center Brute Force: O\(N^2\) / O\(N\)

```python
def productExceptSelf(self, nums: List[int]) -> List[int]:

    result = []

    for i in range(len(nums)):
        product_left = math.prod(nums[:i])
        product_right = math.prod(nums[i+1:])
        result.append(product_left * product_right)

    return result
```

#### 2. O\(n\)/O\(n\)

{% tabs %}
{% tab title="Python" %}
```python
#e.g. nums = [1,2,3,4]

def productExceptSelf(self, nums: List[int]) -> List[int]:
    
    if not nums or len(nums) == 0:
        return 0
        
    f = [0 for _ in range(len(nums))]
    
    # need 1 instead of 0, because 1*x = x
    firstHalf = [1 for _ in range(len(nums))]
    
    # be extra cautious about the range, since i=i-1 could cause out of range.
    # here we skip the first element because of product itself.
    # firstHalf =
    #       ----->
    #i=1 [1, 1, 1, 1] 
    #i=2 [1, 1, 1, 1]
    #i=3 [1, 1, 2, 1]
    #i=4 [1, 1, 2, 6]
    #idx     1  2  3  4
    for i in range(1, len(nums)):
        # transfer function: f[i] = f[i-1] * nums[i-1]
        firstHalf[i] = firstHalf[i-1] * nums[i-1]
    
    
    secondHalf = [1 for _ in range(len(nums))]
    # reversed(range(0, len(nums)-1)) for i = i+1
    # here we skip the last element
    # secondHalf = 
    #           <-----
    #i=3   [1, 1, 1, 1]
    #i=2   [1, 1, 4, 1]
    #i=1  [1, 12, 4, 1]
    #i=0 [24, 12, 4, 1]
    #idx    0  1  2  3   
    for i in reversed(range(0, len(nums)-1)):
        # transfer function: f[i] = f[i+1] * nums[i+1]
        secondhalf[i] = secondHalf[i+1] * nums[i+1]
    
    for i in range(len(nums)):
        f[i] = firstHalf[i] * secondHalf[i]
        
    return f
    
```
{% endtab %}
{% endtabs %}

#### 2. Space Optimized: O\(n\)/O\(1\)

#### 相似題

* Container With Most Water
* Trapping Rain Water I
* Trapping Rain Water II

