# \[Medium\] Longest Increasing Subsequence

## [\[Medium\] Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)      \(6581/151\)

Given an unsorted array of integers, find the length of the longest increasing subsequence.

```text
Ex1
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
```

## Thought Process

根據題意 ‘length of longest....'，我們可以知道是求最長，也就是**求最大/最小值**的變形。  
又因為此題可以把最長的subsequence，化成少一個element + max\(subsequence\)的子問題，因此可以用DP。  
  
此題有多種解法：\(1\)最簡單的是Brute Force，用Recursion一個一個比。  
\(2\)用Recursion，加上memoization紀錄已經算過的答案。  
\(3\)用Dynamic Programming，加上memoization把答案都記下來。  
\(4\)用DP + Binary Search，或是[Patience Sorting](https://en.wikipedia.org/wiki/Patience_sorting)（難想到）

> 思路：  
> Step1. 左右邊界Two pointers掃的方式
>
> ```python
> # e.g. [10, 9, 2, 5, 3, 7, 90, 18]
> #                 R->               R慢慢往右移
> #        L------->|                 找到第一個L符合遞增條件，就停下來
> ```

> Step2. DP Memoization紀錄的方式  
> 從taken or not\_taken兩種，選較長的擇一然後在`DP[i]`紀錄目前最長距離。

### 1. Recursion, Brute Force Scanning: O\(2^N\) / O\(N\)

\[思路\] 因為不知道有多少數字符合，也不知道有多少for loop，因此用Recursion來確認每一個數是不是increasing subsequence裡的一員。  
  
如何確定能否走下去？  
此時只有兩種選擇：\(1\)`taken`  \(2\)`not_taken` 目前的數字nums\[curr\]  
\(1\) `taken`: 取當下的數字nums\[curr\]並且長度+1，再繼續用Recursion往下看能否找到更長的  
       要taken的判斷條件為 `nums[curr] > nums[prev] (向右遞增，越後面數字越大)`  
\(2\) `not_taken`: 不取當下的數字nums\[curr\]，長度維持原樣  
  
1. Recursion\(nums, currIndex, prevIndex\)   
     --&gt; currIndex從**`0`**開始, prevIndex從**`-1`**開始  
2. 建一個lis\_list來記錄LIS的長度  
3. 如果符合條件：\(1\) nums\(currIndex\) &gt; nums\(prevIndex\) or \(2\) prevIndex == -1  
，即lis\_list長度可+1，並且currIndex往後挪，prevIndex變成currIndex。  
4. 如果不符合條件，則currIndex + 1，prevIndex不變。

         **Time Complexity：Ｏ\(** $$2^n$$ **\)**  
         **Space Complexity： Ｏ\(** $$n$$ **\)** ，即`f`，nums數組長度。

```python
def lengthOfLIS(self, nums: List[int]) -> int:
    return self.findMaxLen(nums, -1, 0)

def findMaxLen(self, nums, prev, curr):
    if curr == len(nums):
        return 0

    taken = 0
    # 需要額外確定邊界 prev==-1 時的情況
    # 普通條件：nums[curr] > nums[prev] (向右遞增，越後面數字越大)
    if prev == -1 or nums[curr] > nums[prev]:
        taken = 1 + self.findMaxLen(nums, curr, curr + 1)

    not_taken = self.findMaxLen(nums, prev, curr + 1)

    return max(taken, not_taken)
```

### 2. Recursion + Memoization: O\(N^2\) / O\(N\)

### 3. DP, Bottom-Up: O\(N^2\) / O\(N\)

1. **Define the state:**  原來的數組`[9,2,5,3,7,101]`裡，一定有一個數`a[j]`是整個longest subsequence裡最大的，也肯定有第二大的`a[i]`，即`[ * * *,` **`a[i]`**`, * * *,` **`a[j]`**`]`，  where `j` is always greater than`i (j > i)`  
2.  **Transfer function:**  **`f[j] = max{f[j], f[i] + 1},   where a[j] > a[i]`** `f[j]` 如果為最大，則要+1，否則就是維持原樣`f[j]` 
3. **Init state and set Boundaries**: init state: f\[i\] = 1 boundaries: \(1\)  `i>=0`,   \(2\) `a[j] > a[i]` `a[j]`這個數肯定大於`a[i]` 
4. **Calculate sequence:** f\[0\], f\[1\], f\[2\], ...., f\[n-1\] 裡面其中一個最長的，因此 return max\( f\[0\], f\[1\], f\[2\], ...., f\[n-1\] \)  **Time Complexity：Ｏ\(** $$n^2$$ **\)**，即`for i + for j`，\(i, j\)雙重遍歷。 **Space Complexity： Ｏ\(** $$n$$ **\)** ，即`f`，nums數組長度。

{% tabs %}
{% tab title="Python" %}
```python
# e.g. [10, 9, 2, 5, 3, 7, 90, 18]
#                 R->               R慢慢往右移
#        L------->|                 找到第一個L符合遞增條件，就停下來
#
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
{% endtab %}
{% endtabs %}

### 4. DP + Binary Search: O\(NlogN\) / O\(N\)

{% hint style="info" %}
\(1\) 用`tail_num`作DP array，紀錄的是當下掃到長度\(`max_len`\)的最小LIS順序。  
\(2\) 每增加一個num/target時，如何知道`tail_num`要取哪一個數來取代？  
      用Binary Search來找到需要被替換的最小數。e.g.

`max_len = 1, tail_num = [1], num or target = 3  -> Do nothing  
max_len = 2, tail_num = [1,2] num or target = 3  -> Do nothing  
max_len = 3, tail_num = [1,3,4], num or target = 3  -> 換4 -> tail_num = [1,3,3]  
max_len = 4, tail_num = [1,3,5,6], num or target = 3  -> Do nothing`
{% endhint %}

Just explain more about the tail processing example, based on [https://segmentfault.com/a/1190000003819886](https://segmentfault.com/a/1190000003819886)

`e.g. [1,3,5,2,4,8,6]`

For this list, we can have LIS with different length.  
For length = 1, \[1\], \[3\], \[5\], \[2\], \[8\], \[4\], \[6\], we pick the one with smallest tail element as the representation of length=1, which is \[1\]  
For length = 2, \[1,2\] \[1,3\] \[3,5\] \[2,8\], ...., we pick \[1,2\] as the representation of length=2.  
Similarly, we can derive the sequence for length=3 and length=4  
The result sequence would be:  
len=1: \[1\]  
len=2: \[1,2\]  
len=3: \[1,3,4\]  
len=4: \[1,3,5,6\]

According to the logic in the post, we can conclude that:  
\(1\) If there comes another element, 9   
**\(9是裡面最大數，則append到後面\)**  
We iterate all the sequences, found 9 is even greater than the tail of len=4 sequence, we then copy len=4 sequence to be a new sequece, and append 9 to the new sequence, which is len=5: \[1,3,5,6,9\]  
The result is:  
len=1: \[1\]  
len=2: \[1,2\]  
len=3: \[1,3,4\]  
len=4: \[1,3,5,6\]  
**len=5: \[1,3,5,6,9\]  &lt;-只換它**

\(2\) If there comes another 3,  
**\(3是中間數，則把離3最近且較大的中間數4替換掉\)**  
We found len=3 \[1,3,4\], whose tailer is just greater than 3, we update the len=3 sequence tobe \[1,3,3\]. The result is:  
len=1: \[1\]  
len=2: \[1,2\]  
**len=3: \[1,3,3\]    &lt;-只換它**  
len=4: \[1,3,5,6\]

\(3\) If there comes another 0,  
**\(0是最小數，則替換掉len=1的最小數\)**  
0 is smaller than the tail in len=1 sequence, so we update the len=1 sequence. The result is:  
**len=1: \[0\]**          **&lt;-只換它**  
len=2: \[1,2\]  
len=3: \[1,3,3\]  
len=4: \[1,3,5,6\]

**Time Complexity：Ｏ\(** $$nlogn$$ **\) for num in nums 需要 N 時間 \* Binary Search需要 logN 時間 ＝ NlogN**  
**Space Complexity： Ｏ\(** $$n$$ **\)** ，即`f`，nums數組長度。

```python
def lengthOfLIS(self, nums: List[int]) -> int:
    
    # tail_num is an array storing the smallest tail of all increasing 
    # subsequences with length i+1 in tail_num[i].
    tail_num = [0] * len(nums)
    max_len = 0
    
    # 從頭到尾，把每種 '加上num，則tail_num如何變化' 的情況考慮一遍
    for num in nums:
        # Binary Search
        # 不同於正常的Binary Search，是right需要每次都跟著max_len一起走而非len(nums)-1
        # 為什麼right=max_len？
        # 因為我們當下還沒看到len(nums)-1，只看到max_len，
        # 等於是對每增加一個num時的狀況做一次Binary Search (with target)
        # params: left = 0, right = current max length
        #         target = num
        left, right = 0, max_len
        while left != right:
            mid = left + (right - left) // 2
            if tail_num[mid] < num:
                left = mid + 1
            elif tail_num[mid] >= num:
                right = mid

        tail_num[left] = num
        max_len = max(max_len, left + 1)
        print(num, tail_num, max_len)

    return max_len
```

#### Notes

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

