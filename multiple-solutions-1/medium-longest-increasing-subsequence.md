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



```python
def lengthOfLIS(self, nums: List[int]) -> int:

    if not nums:
        return 0

    dp = [1 for _ in range(len(nums))]

    for right in range(0,len(nums)):
        for left in range(0,right):
            if nums[right] > nums[left] and right > left:
                taken = 1 + dp[left]
                not_taken = dp[right]
                dp[right] = max(not_taken, taken)

    return max(dp)
```

### （3）DP + Binary Search

**Time Complexity：Ｏ\(** $$nlogn$$ **\)**  
**Space Complexity： Ｏ\(** $$n$$ **\)** ，即`f`，nums數組長度。

## Full Implementation

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



