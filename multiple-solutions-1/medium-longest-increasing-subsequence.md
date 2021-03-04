# \[Medium\] Longest Increasing Subsequence

## Question

[Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)  
Given an unsorted array of integers, find the length of the longest increasing subsequence.

#### Example

```text
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
```

## Thought Process

根據題意 ‘length of longest....'，我們可以知道是求最長，也就是**求最大/最小值**的變形。  
又因為此題可以把最長的subsequence，化成少一個element + max\(subsequence\)的子問題，因此可以用DP。  
  
此題有多種解法：\(1\)最簡單的是Brute Force，一個一個比。  
\(2\)用Dynamic Programming，加上memoization把答案都記下來。  
\(3\)用DP + Binary Search。

### （1）Brute Force + Recursion

         **Time Complexity：Ｏ\(** $$2^n$$ **\)**  
         **Space Complexity： Ｏ\(** $$n$$ **\)** ，即`f`，nums數組長度。

\[思路\] 因為不知道有多少數字符合，也不知道有多少for loop，因此用Recursion來確認每一個數是不是increasing subsequence裡的一員。  
1. Recursion\(nums, currIndex, prevIndex\)   
     --&gt; currIndex從**`0`**開始, prevIndex從**`-1`**開始  
2. 建一個lis\_list來記錄LIS的長度  
3. 如果符合條件：\(1\) nums\(currIndex\) &gt; nums\(prevIndex\) or \(2\) prevIndex == -1  
，即lis\_list長度可+1，並且currIndex往後挪，prevIndex變成currIndex。  
4. 如果不符合條件，則currIndex + 1，prevIndex不變。

### （2）Bottom-Up DP: O\(N^2\) / O\(N\)

1. **Define the state:**  原來的數組`[9,2,5,3,7,101]`裡，一定有一個數`a[j]`是整個longest subsequence裡最大的，也肯定有第二大的`a[i]`，即`[ * * *,` **`a[i]`**`, * * *,` **`a[j]`**`]`，  where `j` is always greater than`i (j > i)`  
2.  **Transfer function:**  **`f[j] = max{f[j], f[i] + 1},   where a[j] > a[i]`** `f[j]` 如果為最大，則要+1，否則就是維持原樣`f[j]` 
3. **Init state and set Boundaries**: init state: f\[i\] = 1 boundaries: \(1\)  `i>=0`,   \(2\) `a[j] > a[i]` `a[j]`這個數肯定大於`a[i]` 
4. **Calculate sequence:** f\[0\], f\[1\], f\[2\], ...., f\[n-1\] 裡面其中一個最長的，因此 return max\( f\[0\], f\[1\], f\[2\], ...., f\[n-1\] \)  **Time Complexity：Ｏ\(** $$n^2$$ **\)**，即`for i + for j`，\(i, j\)雙重遍歷。 **Space Complexity： Ｏ\(** $$n$$ **\)** ，即`f`，nums數組長度。

```python
# e.g. [10, 9, 2, 5, 3, 7, 90, 18]
#              L  R 
#       [1, 1, 1, 2, 1, 1,  1,  1] not/taken 1 2   
#
#              L     R
#       [1, 1, 1, 2, 2, 1,  1,  1] not/taken 1 2   #取2,3   大於 不取3，只留2或5
#
#              L        R                      
#       [1, 1, 1, 2, 2, 3,  1,  1] not/taken 2 3   #取2,5,7 大於 不取7，只留2,5或2,3
#
#                 L     R                         
#       [1, 1, 1, 2, 2, 3,  1,  1] not/taken 3 3   #取2,5,7和2,3,7相同                  
#       ....
#       ....
#                 L             R
#       [1, 1, 1, 2, 2, 3,  4,  3] not/taken 2 3   #取x,5,7,18 大於 不取18，只留x,5,7 
#
#                    L          R
#       [1, 1, 1, 2, 2, 3,  4,  3] not/taken 3 3   #取3,7,18和3,7,90相同
#
#                       L       R 
#       [1, 1, 1, 2, 2, 3,  4,  4] not/taken 3 4   #取x,x,7,18  大於 不取18，只留x,x,7
def lengthOfLIS(self, nums: List[int]) -> int:

    if not nums:
        return 0
        
    # dp array 紀錄的是到目前為止最長長度
    # 它同時可以用來做從上一個state轉移到下一個state的參考。即dp[i-1] = max(dp[i-1],dp[i]+1)
    dp = [1 for _ in range(len(nums))]
    
    # right每移動一格，就要重新看從(0, right)這個範圍的最長LIS。
    # 在每發現一個left->遞增->right的序列時，和dp[right]比較，如果更長則更新。
    for right in range(0,len(nums)):
        for left in range(0,right):
            if nums[right] > nums[left] and right > left:
                taken = 1 + dp[left]
                not_taken = dp[right]
                dp[right] = max(not_taken, taken)

    return max(dp)
```

{% tabs %}
{% tab title="Python" %}
```python
def lengthOfLIS(self, nums: List[int]) -> int:

    if not nums or len(nums) == 0:
        return 1
        
    f = [1 for _ in range(len(nums))]
    
    for j in range(len(nums)):
        for i in range(j): # in this case, j is always greater or equal than i
            if nums[j] > nums[i]:
                f[j] = max(f[j], f[i] + 1)
                
    return max(f)
```
{% endtab %}
{% endtabs %}

### （3）DP + Binary Search

**Time Complexity：Ｏ\(** $$nlogn$$ **\)**  
**Space Complexity： Ｏ\(** $$n$$ **\)** ，即`f`，nums數組長度。





```text
 '''
1. define the state
[~~~, a[i], ~~~, a[j]]
to get LIS, there's always a LIS result f[j] where the sequence ends with a[j].
before a[j], there should be another number a[i] where it is the second largest compare to a[j]. 

# Note: j is always greater than i (j>i)

2. transfer function
f[j] = max{1 or f[i] + 1 (where j>i and a[j]>a[i])}

3. init state and set boundaries

init state: f[i] = 1
boundaries: 
    (1) i >= 0
    (2) a[j] > a[i]

4. calculate sequence
f[0], f[1], f[2], ... f[n-1] 
then return max{ f[0], f[1], f[2], ... f[n-1] }

time complexity:  n^2
space complexity: n      
```

