# \[Medium\] Next Greater Number III

Next Greater Number III  
Given a positive integer `n`, find _the smallest integer which has exactly the same digits existing in the integer_ `n` _and is greater in value than_ `n`. If no such positive integer exists, return `-1`.

**Note** that the returned integer should fit in **32-bit integer**, if there is a valid answer but it does not fit in **32-bit integer**, return `-1`.

**Example:**

```text
Example1:
Input: n = 12
Output: 21

Example2:
Input: n = 21
Output: -1

Example3:
Input: n = 230241
Output: 230412

Example4:
Input: n = 12388123
Output: 12388132

Example5:
Input: n = 1999999999
Output: -1 (over 32-bit integer)
```

## **Code**

### 1. Two Pointer \(-&gt;\|&lt;-\): O\(n\)/O\(n\)

和下面的next\_permutation很像，但有些edge cases需要額外處理。

{% page-ref page="medium-next-permutation.md" %}

Step1: 把所有數字換成list of integers。\(e.g. 12 --&gt; \[1,2\] \)  
             NOTE: 需要做兩次type轉換。int\(\) --&gt; str\(\) --&gt; int\(\)  
  
Step2: 用兩個pointers, `left = len(nums)-1, right = -1`, left & right來找應該要調換的兩個數字的index，left是從右邊一個一個往左看，找到`nums[left-1] < nums[left]`時便停止並且用right pointer標記。  
  
Step3: 此時我們有left&right pointers了，我們可以先把這兩個數字調換，並且把中間的所有數字，從`nums[right+1:]`依照小至大的順序排列\(sorted\)。  
  
Step4: 把所有的數字用`''.join(nums list)`粘起來。當然還是要做型態轉換。  
  
Step5: 額外處理32-bit digits limit's edge case。   


```python
def nextGreaterElement(self, n: int) -> int:
    if not n:
        return -1
    
    # convert n into int list
    # or) nums = [int(i) for i in str(n)]
    nums = list(str(n))
    
    # edge case like `1`
    if len(nums) == 1:
        return -1
    
    # towards LEFT  start       <---- end  
    left = len(nums)-1
    # towards RIGHT start --->        end
    right = -1
    
    while left > 0:
        if nums[left - 1] < nums[left]:
            right = left - 1
            break
        left -= 1
    
    # edge case like `21`
    # where right pointer never moves
    if right == -1:
        return -1
    
    # swap all digits from left pointer, and we are going backwards 
    for left in range(len(nums))[::-1]:
        if nums[left] > nums[right]:
            nums[left], nums[right] = nums[right], nums[left]
            # reverse/sort all the remaining digits to make it smallest
            # or) nums[right+1:] = sorted(nums[right+1:])
            nums[right+1:] = nums[right+1:][::-1]
            break
    
    # assemble all digits back into integer
    result = int(''.join(str(i) for i in nums))
    
    # edge case 32-bit integer case
    32_limit = 2**31-1
    if result > 32_limit:
        return -1
    return result 
```

