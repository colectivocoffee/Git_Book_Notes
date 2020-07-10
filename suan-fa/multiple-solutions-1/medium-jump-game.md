# \[Medium\] Jump Game

## Question

[Jump Game](https://leetcode.com/problems/jump-game/)  
Given an array of non-negative integers, you are initially positioned at the first index of the array.  
Each element in the array represents your maximum jump length at that position.  
Determine if you are able to reach the last index.

## Thought Process

可以化成子問題，因此可以選擇用（1）DP  
可以使用貪心思想，實時維護最遠可達位置，因此使用（2）Greedy Algo\(貪心算法\)

{% hint style="info" %}
注意：貪心算法常靠背答案，因此如果能用DP的題目來解，就儘量用DP。
{% endhint %}

### （1）DP

時間複雜度Ｏ\( $$n^2$$ \)：n為list數組長度，遍歷數組裡每一個element，並且遍歷每種可能的步數，即雙重遍歷。  
空間複雜度Ｏ\( $$n$$ \)：建立`dp[]` 時長度為n的數組。  
  
\# DP Solution Cons：  
當list數組裡都是1時\( \[1,1,1,1,1,1,.....1\] \)，此解法就會超時TLE \(Time Limit Exceeding\)。  
  
\# How to Improve：  
\(1\)把遍歷改成backwards `[ : :-1]`，會比從前到後更省時間。  
\(2\)如果可以跳到點 i \(`f[i] == True`\)，肯定也能跳到 i 前面任何一點，因此 i  前面的任何一點都可以設為跟 i 一樣為True。  


### （2）Greedy 

時間複雜度Ｏ（ $$n$$ ）：遍歷數組。  
空間複雜度Ｏ（ $$1$$ ）：建立變量destination and source。

## Full Implementation

#### \(1\) DP：

```python
"""
@param nums: list of integers
@return: A boolean
"""
def canJump(self, nums): 
    if not nums:
        return False
    
    f = [False] * len(nums)
    f[0] = True
    
    for j in range(1, len(nums)):
        for i in range(j):
            if f[i] == True and i + nums[i] >= j:
                f[j] = True
                break
    
    return f[len(nums)-1]
    
```

**\(2\) Greedy：**  
Moving from the end of the array and try to reach the beginning. At the end of the destination should be equal to 0 as we need to start from the beginning. 

```python
"""
@param nums: list of integers
@return: A boolean
"""
def canJump(self, nums): 
    # moving from the end of the array.
    destination = len(nums)-1
    source = destination-1
    
    while source >= 0:
        if source + nums[source] >= destination:
            destination = source
            source -= 1
        else:
            source -= 1
    
    return destination == 0
```

  


