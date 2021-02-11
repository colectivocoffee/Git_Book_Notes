# \[Medium\] Jump Game /                                       \[Hard\] Jump Game II

## \[Medium\] Jump Game

[Jump Game](https://leetcode.com/problems/jump-game/) \(5636/393\)  
Given an array of non-negative integers, you are initially positioned at the first index of the array.  
Each element in the array represents your maximum jump length at that position.  
Determine if you are able to reach the last index.

## Thought Process

要如何跳到最後一格？  
1\) 如果要成功，我們只能確定最後一格已跳到`(Base Case)`  
2\) Question: 那我們怎麼跳到最後一格呢？必須要有上一格`(prev_pos)`有足夠的步數，才能跳到最後一格。那問題就再度變成Question：我們要如何跳到上一格呢？必須要有上上一格....  
3\) 如此重複，直到我們回到第一格`nums[0]`為止，即為成功。

\[關鍵\] 檢查是否能跳到下一格的辦法為：  
       從curr跳到prev  
       **prev              &lt;----              curr**  
`if curr + nums[curr] >= last_pos:  
    last_pos = curr`

可以化成子問題，因此可以選擇用（1）DP Bottom-Up \(2\) DP Top-Down  
可以使用貪心思想，實時維護最遠可達位置，因此使用（3）Greedy Algo\(貪心算法\)

### 1. Backtracking/Recursion Start-&gt;End: O\(2^N\) / O\(N\)

We start from the first position and jump to every index that is reachable. We repeat the process until the last index is reached. When stuck, backtrack.  
  
Time Complexity: **O\(** $$2^N$$**\)** There are 2^N \(upper bound\) ways of jumping from the first position to the last.  
Space Complexity: **O\(N\)** 使用Recursion需要O\(N\)的Stack extra memory

```python
def canJump(self, nums: List[int]) -> bool:  
    return self.fromPos(0, nums)

def fromPos(self, curr, nums):
    # Recursion Exit條件: 跳到最後一格       
    if curr == len(nums) - 1:
        return True
    
    # furtherest_jump代表的是: 目前所在位置(curr) + 目前可跳步數(nums[curr])
    # 然而，有可能會超過最後一步(len(nums)-1)，因此我們用min(fur.. , last_step)來避免
    furtherest_jump = min(curr + nums[curr], len(nums) - 1)
    print(curr, furtherest_jump)
    # 不包頭(curr)包尾，因此是range(curr+1, furtherest_jump+1)
    for next_pos in range(curr + 1, furtherest_jump + 1):
        # 如果卡住，則backtrack
        if self.fromPos(next_pos, nums):
            #如果跳到最後，則成功
            return True
    return False
```

### 2. DP Top-Down 從頭掃到尾: O\(N^2\) / O\(N\)

時間複雜度Ｏ\( $$n^2$$ \)：n為list數組長度，遍歷數組裡每一個element，並且遍歷每種可能的步數，即雙重遍歷。  
空間複雜度Ｏ\( $$n$$ \)：建立`dp[]` 時長度為n的數組。  
  
\# DP Solution Cons：  
當list數組裡都是1時\( \[1,1,1,1,1,1,.....1\] \)，此解法就會超時TLE \(Time Limit Exceeding\)。  
  
\# How to Improve：  
\(1\)把遍歷改成backwards `[ : :-1]`，會比從前到後更省時間。  
\(2\)如果可以跳到點 i \(`f[i] == True`\)，肯定也能跳到 i 前面任何一點，因此 i  前面的任何一點都可以設為跟 i 一樣為True。

```python
# TLE Not passed
def canJump(self, nums): 
    if not nums:
        return False
    
    f = [False] * len(nums)
    f[0] = True
    
    for furtherest in range(1, len(nums)):
        for curr in range(furtherest):
            if f[curr] == True and curr + nums[curr] >= furtherest:
                f[furtherest] = True
                break
    
    return f[len(nums)-1]
    
```

### 3. DP Bottom-Up 從尾往回頭: O\(N\) / O\(1\)

> 思路：按題目要求，我們如果能跳到最後一格last\_pos，意味著可以從最後一格往前跳，看看是否能跳回第一格start。  
> step1. **從尾往頭跳**，可以用DP的思維去想。  
>   
> step2. **定義Base Case**：令最後一格base case，`last_pos = len(nums)-1`。即跳成功的話，不管怎樣，都能跳到`last_pos`這個位置上。  
>   
> step3. **定義往回跳的方法**：`for i in range(len(nums)-2, -1, -1)`，注意這裡原來是len\(nums\)-1，但因為inclusive，還要再減1。  
>   
> step4. **\[關鍵\] 如何從`last_pos`跳回上一個`preceding_index`**： 我們必須要擁有足夠的jump nums，才能跳到`last_pos`，否則不管怎樣都跳不到尾。  
> 意即`preceding_idx + jump num >= last_pos`，並且把`last_pos`變成`preceding_index`，繼續往回走。  
> 然後重複這一步驟，直到回到頭`nums[0]`。  
>   
> step5. **如果碰到indics為0怎麼辦？**   
> 0的話代表著sinkhole or barriers that can go thru  
> \(1\)**sinkhole** : 剛好跳到sinkhole，不管怎樣都會跳不出來，只能return False。  
> e.g. `nums=[3,2,1,0,4]   #這裡0是sinkhole`  
> \(2\)**barriers that can go thru** : 即使碰到0，也可以用jump num的數字跳過0，因此不受影響。  
> e.g. `nums=[3,2,2,0,4]   #這裡0只是barrier且可以跳過`  
>   
> step6. **判斷最後一格是否回到`index 0`：**即 `last_pos == 0`

Here's the thinking proces:

* Base case: last index can trivially reach to last index.
* **Q1**: How can I reach to the last index \(I will call it `last_position`\) from a preceding index?
  * If I have a preceding index `idx` in `nums` which has jump count `jump` which satisfies `idx+jump >= last_position`, I know that this `idx` is good enough to be treated as the last index because all I need to do now is to get to that `idx`. I am going to treat this new `idx` as a new `last_position`.
* I ask **Q1** again.

```python
def canJump(self, nums: List[int]) -> bool:

    last_pos = len(nums) - 1
    
    # 從尾巴往回掃有下面兩種寫法
    # 1. range(len(nums)-2, -1, -1)    注意：都是把長度再減1
    # 2. range(len(nums)-1)[::-1]
    # 為什麼要把長度再減1呢？
    # 由於我們是從第一格開始跳，跳到最後一格(inclusive)，因此需要少算1。
    # 
    # for i in range(len(nums)-2, -1, -1):
    for i in range(len(nums)-1)[::-1]:
        if i + nums[i] >= last_pos:
            last_pos = i

    return last_pos == 0
```

### **4. Greedy: O\(N\) /O\(1\)**

時間複雜度Ｏ（ $$n$$ ）：遍歷數組。  
空間複雜度Ｏ（ $$1$$ ）：建立變量destination and source。

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

#### Reference

1. [https://leetcode.com/problems/jump-game/discuss/596454/Python-Simple-solution-with-thinking-process-Runtime-O\(n\)](https://leetcode.com/problems/jump-game/discuss/596454/Python-Simple-solution-with-thinking-process-Runtime-O%28n%29)
2. 
## [\[Hard\] Jump Game II](https://leetcode.com/problems/jump-game-ii/)      \(3629/168\)

