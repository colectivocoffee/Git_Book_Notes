# \[Medium\] Next Permutation

[Next Permutation](https://leetcode.com/problems/next-permutation/)  
Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order \(ie, sorted in ascending order\).

The replacement must be [**in-place**](http://en.wikipedia.org/wiki/In-place_algorithm) and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

`1,2,3` → `1,3,2`  
`3,2,1` → `1,2,3`  
`1,1,5` → `1,5,1`

## Thought Process

### 1. Two Pointers \(-&gt;/&lt;-\)

Next Permutation的意思就是比找比原來大一個的排列。舉個例子：  
`1 2 7 4 3 1` -&gt; `1 3 1 2 4 7`

而我們要如何得到Next Permutation呢？透過觀察我們可以發現，如果從最後往前看，數字會慢慢變大，直到`2`才又減小。下一步是從後往前，找第一個比`2`大的數字，即`3`，把2和3調換，並且把後面所有的數字按照最小遞增的方式排列，即完成。步驟如下：

```python
                                # 建立兩個指針，left&right，用left找'2'
1　 '2'　　7　　4　　 3　　 1      # 從後往前找到2，第一個開始遞減的數字 
1　 '2'　　7　　4　　'3'　　1      # 找到第一個比2大的數字，即3，用right指針標註
1　 '3'　　7　　4　　'2'　　1      # 將2和3調換
1    3　 '1'  '2'　'4'　 '7'     # 把3後面所有的數字按照升序排列，swap left&right + sorted()
```

Q: 為什麼要從最右邊開始看\(start from right end\)呢？  
Ans: 因為我們要取least significant digit that is greater than the current number.  
  
Q:為什麼要換right & left pointers呢？right & left 什麼時候該停下來？  
Ans: 我們只要找一個 "比nums\[i-1\]大一點點的數字"，來當作替換的數字。so that this can become new number and is 1 greater than the new one  
  
Q:為什麼還要reverse/sorted呢？  
Ans: 我們要找到next permutation, 意即這個數字必須要盡量小，越小越好。  
在Step1時，`'2'  7  4  3  1` 是越往後遞減的，  
而當我們換完left&right的數字後，就需要把剩下的數字，按照越往後遞增的方式排列。  


## Code

```python
def nextPermutation(self, nums: List[int]) -> None:
    if not nums:
        return
        
    left = len(nums)-1    # to accomodate case like [4,3,2,1] that needs to be reversed entirely.
    right = -1
    
    # going backwards
    # [2,1,4,3]
    #    <-- left    
    while left > 0:
        # found first element(nums[left-1]) where it is smaller than prev one(nums[left])
        if nums[left - 1] < nums[left]:
            # mark the first element index using right pointer
            right = left-1
            break
        left -= 1
    
    # swap everything from the end to the first element
    for left in range(len(nums))[::-1]:
        # all elements after first element
        if nums[left] > nums[right]:
            # then swap
            nums[right], nums[left] = nums[left], nums[right]
            # then sort the remaning ones into ascending order
            nums[right+1:] = sorted(nums[right+1:])
            return
    
```

