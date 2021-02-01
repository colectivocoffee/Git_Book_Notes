# \[Hard\] First Missing Positive / \[Easy\] Missing Number / \[Medium\] Find the Duplicate Number

## [\[Hard\] First Missing Positive  \(5063/922\)](https://leetcode.com/problems/first-missing-positive/)

Given an unsorted integer array `nums`, find the smallest missing positive integer.

**Follow up:** Could you implement an algorithm that runs in `O(n)` time and uses constant extra space?

## Thought Process

There are only two possibilities/cases:

1. there is no missing integer in the array. e.g. \[1,2,3,4,5\] , \[7,8,9,10,11\]
2. there is a missing integer in the array. e.g.  \[-1,1,2,4,5\] , \[7,8,10,11,12\]

**Case1: There is** _**no**_ **missing integer**   
This means that the array has all number from 1 to n. This must mean that the array is full.   
\(1\) If we encounter case like \[1,2,3,4,5\], we then need to return n+1, which is the first smallest integer.  
\(2\) If we encounter case like \[7,8,9,10,11\], we just have to return 1.   
  
**Case2: There is** _**a**_ **\(or more\) missing integer**   
This means that the missing one must be in the range \[1... n\]. Otherwise, it must be Case1 where there's no space to fill in the missing one.  
\(1\) If we encounter case like \[-1,1,2,4,5\], we then need to find the first cell not marked  
\(2\) If we encounter case like \[7,8,10,11,12\], we just have to return 1. 

需要想到的edge cases：

* `[]`
* `[1]`
* `[0]`
* `[7,8,9,11,12]`

### 1. `Sorted` + `smallest = 1`: O\(NlogN\) / O\(1\)

```python
def firstMissingPositive(self, nums: List[int]) -> int:
    
    nums = sorted(nums)
    # 易錯點：
    # smallest在update之前一直會是前一個數。
    # 並且按題意要求，如果最小的數都比1大 or nums為空，則返回1       
    smallest = 1
    
    for num in nums:
        if num == smallest: 
            smallest += 1        
    return smallest 
```

### 2. Swap with index+1 : O\(N\) / O\(1\)

\(1\) Ignore all numbers &lt;=0 and &gt;n since they are outside the range of possible answers \(which we proved was \[1..n\]\). Case1 here.  
\(2\) For all other integers &lt; n+1, swap them with its correct index + 1.   
     e.g. `[3,4,-1,1]` -&gt; `[-1,4,3,1]`then 3 should be at index2.    
\(3\) Find the first cell not in the correct index order, that is the first missing integer.   
 If there's no unindexed cell, there was no missing integer \(full list\), so return n+1. In this case, we cover situation like  \[7,8,10,11,12\] and \[1,2,3,4,5\]

![](../../.gitbook/assets/image%20%289%29.png)

```python
def firstMissingPositive(self, nums: List[int]) -> int:
    
    # nums = []
    if not nums:
        return 1
    
    
    i = 0    
    # Swap + indexing+1
    # 把所有的數字用swap + index的方式，按照[1,2,3,4...]順序排列。
    while i < len(nums):
        
        # 關鍵點:
        # keep swapping
        # nums[i]為正數，放在i+1位置
        # 假设交换的数据还是大于0且<i+1，则放在合适的位置,且数据不相等，避免死循环
        # nums[prev]代表的是，上一個不在正確位置上的數
        prev = nums[i] - 1
        if 0 <= prev < len(nums) and nums[i] != nums[prev]:
            nums[i], nums[prev] = nums[prev], nums[i]
                 
        else:
            i += 1
    
    # case1, [7,8,9,10] array is full, and first integer nums[0] is not 1, 
    #        then return i+1 (0+1) = 1         
    # case2, [3,4,-1,1] -> [1,-1,3,4] Find the missing integer in this array.
    for i in range(len(nums)):
        if nums[i] != i + 1:
            return i + 1
    
    # case1
    # no positive numbers were found, 
    # which means the array contains all numbers 1..n
    return len(nums) + 1
```

**References**  
1.[https://www.cnblogs.com/clnchanpin/p/6727065.html](https://www.cnblogs.com/clnchanpin/p/6727065.html)  
2.[https://leetcode.com/problems/first-missing-positive/discuss/319270/Explanation-of-crucial-observation-needed-to-deduce-algorithm](https://leetcode.com/problems/first-missing-positive/discuss/319270/Explanation-of-crucial-observation-needed-to-deduce-algorithm)

## [\[Easy\] Missing Number](https://leetcode.com/problems/missing-number/) \(2504/2431\)

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return _the only number in the range that is missing from the array._

**Follow up:** Could you implement a solution using only `O(1)` extra space complexity and `O(n)` runtime complexity?

### 1. Sorted: O\(NlogN\) / O\(1\)

```python
def missingNumber(self, nums: List[int]) -> int:
    
    nums = sorted(nums)
    
    for i in range(len(nums)):
        if i not in nums:
            return i
       
    # 由於index會比真實數少1，因此missing number會是len(nums)
    # e.g [0,1] -> 2 = len(nums)
    return len(nums)
```

### 2. Swap with index+1 : O\(N\) / O\(1\)

思路和First Missing Positive相同。  
用swap with index+1的方式去換每一個數，把每一個數都放在index+1的位置上。注意要處理nums\[0\] != 0的情況。

```python
def missingNumber(self, nums: List[int]) -> int:
    
    i = 0 
    # 最重要的while loop，用swap的方法換數字。
    while i < len(nums):
        prev = nums[i] - 1
        if 0 <= prev < len(nums) and nums[i] != nums[prev]:
            nums[i], nums[prev] = nums[prev], nums[i]
        else:
            i += 1
    
    for i in range(len(nums)):
        if nums[i] != i+1:
            return i+1
    
    # 易錯點
    if nums[0] != 0:
        return 0
    return len(nums)

```

### 3. Math, **Gauss' Formula: O\(N\) / O\(1\)**

Gauss' Formula: $$\sum\limits_{i=0}^n i = \frac{n(n+1)}2{}  $$   
The number that is missing is simply the result of Gauss' formula minus the sum of `nums`, as `nums` consists of the first n natural numbers minus some number.

Space Complexity: O\(1\) This approach only pushes a few integers around, so it has constant memory usage.

```python
def missingNumber(self, nums: List[int]) -> int:
        
    n = len(nums)   
    expected_sum = n*(n + 1) // 2
    actual_sum = sum(nums)    
    return expected_sum - actual_sum
```

### 4. HashSet: O\(N\) / O\(N\)

```python
def missingNumber(self, nums: List[int]) -> int:
    
    num_set = set(nums)
    n = len(nums) + 1
    
    for num in range(n):
        if num not in num_set:
            return num
```

### 5. Bit Manipulation: **O\(N\) / O\(1\)**

```python
def missingNumber(self, nums: List[int]) -> int:
    
    missing = len(nums)    
    for i, num in enumerate(nums):
        missing ^= i ^ num        
    return missing
```

## [\[Medium\] Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) \(6651/717\)

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.  
There is only **one repeated number** in `nums`, return _this repeated number_.

### 1. Sorted: O\(NlogN\) / O\(1\)

```python
def findDuplicate(self, nums: List[int]) -> int:

    nums = sorted(nums)
    for i in range(len(nums)):
        if nums[i] == nums[i-1]:
            return nums[i]
```

### 2. Dictionary: O\(N\) / O\(N\)

```python
def findDuplicate(self, nums: List[int]) -> int:
    
    duplicate = {}
    for num in nums:
        duplicate[num] = duplicate.get(num, 0) + 1
        
    for num, count in duplicate.items():
        if count != 1:
            return num
```

### 3. Empty Set: O\(N\) / O\(N\) 

```python
def findDuplicate(self, nums: List[int]) -> int:

    nums_set = set()       
    for num in nums:
        if num in nums_set:
            return num
        nums_set.add(num)
```

**Follow Up:** Can you solve the problem using only constant, `O(1)` extra space? Then Use Sol.4.

### 4. Linked List Cycle \(Cycle Detection\): O\(N\) / O\(1\)

![](../../.gitbook/assets/image%20%2811%29.png)

![](../../.gitbook/assets/image%20%2810%29.png)

![](../../.gitbook/assets/image%20%288%29.png)

```python
def findDuplicate(self, nums):
    # Find the intersection point of the two runners.
    tortoise = hare = nums[0]
    while True:
        tortoise = nums[tortoise]
        hare = nums[nums[hare]]
        if tortoise == hare:
            break

    # Find the "entrance" to the cycle.
    tortoise = nums[0]
    while tortoise != hare:
        tortoise = nums[tortoise]
        hare = nums[hare]

    return hare
```

## [\[Easy\] Single Number](https://leetcode.com/problems/single-number/) \(5639/187\)

Given a **non-empty** array of integers`nums`, every element appears _twice_ except for one. Find that single one.

**Follow up:** Could you implement a solution with a linear runtime complexity and without using extra memory?

### 1. Sorted: O\(NlogN\) / O\(1\)

```python
#   e.g. [4, 1, 2, 1, 2, 5, 5]
# sorted [1, 1, 2, 2, 4, 5, 5]
# index i    1^
#                  3^ 
#                        5^
# ans = 4
def singleNumber(self, nums: List[int]) -> int:

    nums = sorted(nums)        
    
    # range(start, stop, step) 每次跳兩格
    for i in range(1, len(nums), 2):   
             
        if nums[i] != nums[i-1]:
            # nums[i]和前(i-1) 後(i+1)都不一樣，nums[i] is unique 
            if nums[i] != nums[i+1]: 
                return [i]
            # condition: nums[i] == nums[i+1]
            # 意味nums[i-1]是unique               
            return nums[i-1]
    # condition: (1)nums只有一個數 (2)最後一個數是unique
    return nums[-1]
```

### 1. Dictionary: O\(N\) / O\(N\)

```python
def singleNumber(self, nums: List[int]) -> int:

    if not nums:
        return 0

    dictionary = {}
    for num in nums:
        dictionary[num] = dictionary.get(num, 0) + 1

    for num, count in dictionary.items():
        if count == 1:
            return num
```

