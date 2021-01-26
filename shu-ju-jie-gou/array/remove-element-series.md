# Remove Element Series

## Remove Element Series

通常這種考題都是需要Space Complexity `O(1)`，不能建新的array/list，我們只能修改原始nums使其符合條件。正因為如此，如果直接在原始`nums.remove(element)`的時候會報錯，我們不能同時移除＆同時loop thru elements。

那該如何做呢？這時候two pointers便派上用場。  
\(1\) 同向雙指針\(-&gt;/-&gt;\)  
\(2\) 相向雙指針\(-&gt;/&lt;-\)

## [\[Easy\] Remove Element](https://leetcode.com/problems/remove-element/) \(1868/3259\)

> Given an array _nums_ and a value `val`, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.  
> Do not allocate extra space for another array, you must do this by **modifying the input array** [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) with `O(1)` extra memory.  
> The order of elements can be changed. It doesn't matter what you leave beyond the new length.

### 1. Two pointers \(-&gt;/-&gt;\): O\(N\) / O\(1\)

 Time Complexity: O\(N\) both idx and index\(num\) traverses 2N steps at most. 

由於只能使用現有的memory，我們必須要modify the input array in-place。因此我們使用`快慢指針`來完成。  
為什麼用快慢指針呢？因為我們可以讓快指針\(`index(num)`\)往後看，讓num跟val比較；而慢指針\(`idx`\)用來remove element。

```python
# e.g. nums=[3,2,2,3]  val=3
# -> [2,2,3,3]
        ^
        idx 
def removeElement(self, nums: List[int], val: int) -> int:
    
    idx = 0
    for num in nums:  
#            print(idx, nums[idx], num, nums)
        if num != val:                
            nums[idx] = num
            idx += 1
    
    return idx
```

```python
# e.g. [ 3, 2, 2, 3]
#      s,f              [3,2,2,3]
#        s  f           [3,2,2,3]
#           s  f        [2,2,2,3]
#              s  f     [2,2,2,3]
# ans= [ 2, 2]
def removeElement(self, nums: List[int], val: int) -> int:
    slow = 0    
    for fast in range(len(nums)):
        if nums[fast] != val:
            nums[slow] = nums[fast]
            slow += 1
    return slow
```

### 2. Two pointers \(-&gt;/&lt;-\): O\(N\) / O\(1\)

Time Complexity: O\(N\)  both left & right traverse N times. Would be slightly more efficient than prev one.

另一種方法，則是用反向指針。一個從頭`left = 0`另一個從尾`right = len(nums)` 開始走，當我們左指針碰到val時，nums\[left\] == val，則 swap，意即`nums[left] = nums[right - 1]` 並且把right往前挪一格。  
當兩個指針碰在一起時即結束。

![](../../.gitbook/assets/image%20%286%29.png)

```python
# e.g. [ 3, 2, 2, 3]
#        l           r   [3,2,2,3]
#        l        r      [3,2,2,3]
#        l     r         [2,2,2,3]
#           l  r         [2,2,2,3]
# ans= [ 2, 2]
def removeElement(self, nums: List[int], val: int) -> int:
    
    left, right = 0, len(nums)
    
    while left < right:
        if nums[left] == val:
            nums[left] = nums[right - 1]
            right -= 1
        else:
            left += 1
    
    return right
```

## [\[Easy\] Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)   \(3430/6340\)

> Given a sorted array _nums_, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appears only _once_ and returns the new length.
>
> Do not allocate extra space for another array, you must do this by **modifying the input array** [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) with O\(1\) extra memory.

### 1. Two Pointers \(-&gt;/-&gt;\): O\(N\)/O\(1\)

用fast&slow index looping。快指針指的是要比較的值；而慢指針

```python
# e.g.   [0, 0, 1, 2, 2, 2, 3]
#                                        nums            nums[f] != nums[s]
#    0 1  s  f                     [0, 0, 1, 2, 2, 2, 3]
#    0 2  s     f                  [0, 0, 1, 2, 2, 2, 3]  !=
#    1 3     s     f               [0, 1, 1, 2, 2, 2, 3]  !=
#    2 4        s     f            [0, 1, 2, 2, 2, 2, 3]
#    2 5        s        f         [0, 1, 2, 2, 2, 2, 3]
#    2 6        s           f      [0, 1, 2, 2, 2, 2, 3]  !=
#    3 6           s        f      [0, 1, 2, 3, 2, 2, 3]
#    ->  [0, 1, 2, 3|....]
def removeDuplicates(self, nums: List[int]) -> int:
    
    slow = 0
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            # 如果不相同，則把慢指針往後移一位，並且更新慢指針的值（從快指針得到）
            slow += 1
            nums[slow] = nums[fast]
    
    return slow + 1
```

### 2. Two Pointers \(-&gt;/-&gt;\): O\(N\) / O\(1\)

**idx + prev + num**  
idx 代表slow pointer，而num代表fast pointer的值。我們用prev來儲存上一個值，用來比較用。但本質上還是two pointers。

```python
# idx num prev         nums
# 0   0   None [0, 0, 1, 2, 2, 2, 3]
                ^
# 1   0   0    [0, 0, 1, 2, 2, 2, 3]
                   ^
# 1   1   0    [0, 0, 1, 2, 2, 2, 3]
                   ^
# 2   2   1    [0, 1, 1, 2, 2, 2, 3]
                      ^
# 3   2   2    [0, 1, 2, 2, 2, 2, 3]
                         ^
# 3   2   2    [0, 1, 2, 2, 2, 2, 3]
                         ^
# 3   3   2    [0, 1, 2, 2, 2, 2, 3]
                         ^
# 4   3   3    [0, 1, 2, 3, 2, 2, 3]
                            ^
def removeDuplicates(self, nums: List[int]) -> int:
    
    idx = 0
    prev = None
    
    for num in nums:
        if num != prev:
            nums[idx] = num
            prev = num
            idx += 1
    
    return idx
```

### 3. `Set()+Sorted()+[:]` : O\(N\) / O\(1\)

* `sorted()` 這個方法是對nums本身 in-place 排序。
* `nums[:]`   Slice assignment，直接replace原本的nums。

```python
def removeDuplicates(self, nums: List[int]) -> int:
    
    nums[:] = sorted(set(nums))
    return len(nums)
```

## [\[Medium\] Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)  \(1674/767\)

### 1. Two Pointers \(-&gt;/-&gt;\): O\(N\) / O\(1\)

while loop 寫法。

```python
# slow,fast,         nums
#   2    2      [1, 1, 1, 2, 2, 3]
                 ^    s,f
#   2    3      [1, 1, 1, 2, 2, 3]    !=
                 ^     s  f
#                |<- 3  ->|    => then remove nums[slow-2]
 
#   3    4      [1, 1, 2, 2, 2, 3]    != 
                    ^     s  f
#   4    5      [1, 1, 2, 2, 2, 3]    !=
                       ^     s  f
#   5    6      [1, 1, 2, 2, 3, 3]    !=
                          ^     s  f
def removeDuplicates(self, nums: List[int]) -> int:
    
    slow, fast = 2, 2
    
    if len(nums) < 2:
        return len(nums)
    
    while fast < len(nums):
        # 為什麼是slow - 2 ?
        # 由於題目要求允許有重複數2個，加上fast pointer本身，
        # slow-2和fast的'距離超過3'，則應該要剔除nums[slow]
        if nums[slow - 2] != nums[fast]:
            nums[slow] = nums[fast]
            slow += 1
        fast += 1
    return slow
```

for loop 寫法。

```python
def removeDuplicates(self, nums: List[int]) -> int:
    
    slow, fast = 2, 2
    
    if len(nums) < 2:
        return len(nums)
    
    # 注意range是從'2'開始，是因為需要slow - 2 才不會out of range。
    # 並且corner case已完成。前面兩個不管發生什麼事，長度都會是2以下。
    for fast in range(2, len(nums)):
        if nums[slow - 2] != nums[fast]:
            nums[slow] = nums[fast]
            slow += 1

    return slow
```

