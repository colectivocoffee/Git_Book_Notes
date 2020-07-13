# Binary Search

如果題目說  
\(1\)要O\(logn\)的複雜度，有很大機率是用Binary Search \(二分法\) 求解。當然，如果array裡都是duplicates，就會從O\(logn\) -&gt; O\(n\)，因為需要遍歷所有elements。

\(2\)array是排序的，即Rotated Sorted Array/Sorted Array，又要求fast/efficient，那就是BinarySearch。

## While Loop or Recursion?

不要自行判斷，先跟面試官討論！

判斷是否用Binary Search Recursion的條件：  
\(1\) Recursion深度是否會很深  
\(2\) 是否面試官要求不能用Recursion

## Binary Search 模板

a. 通過`while loop (start + 1 < end)`，將start/end範圍縮小到n=2，即`[start, end]`。  
b. 在第二步`if nums[start]/nums[end] == target`來判斷屬於start還是end，如果都不符合則返回-1。 

### \(1\) While Loop \(start/end\)

```python
start, end = 0, len(nums)-1

# use 'start + 1 < end' to prevent dead lock
while start + 1 < end:
    
    mid = start + (end-start)//2
    
    # mid < end && mid > start
    if nums[mid] < nums[end]:
        end = mid
    elif nums[mid] > nums[start]:
        start = mid
    # nums[mid] == nums[start]
    else: 
        start = mid

# since we use start + 1 < end, start&end will be /start,end/ relationship.
# choose first position
# [start,end] 
     ^
if nums[start] < nums[end]:
    return nums[start]

# choose second position
# [start, end]
           ^
else:
    return nums[end]
```

### \(2\) While Loop \(with target\)

```python
start, end = 0, len(nums)-1

#### start + 1 < end ####
while start + 1 < end:
    
    mid = start + (end-start)//2
    
    #1.
    if nums[mid] < target:
        start = mid
    #2.
    elif nums[mid] > target:
        end = mid
    #3. nums[mid] == target
    else: 
        end = mid

#[start, end]
# to handle start + 1 < end, choose first one
if nums[start] == target:
    return start
# choose second one
if nums[end] == target:
    return end
    
# can't find result
return -1
```

### \(3\) Recursion 模板

```python
def findIndex(self, nums, target):
    return self.binarySearch(nums, 0, len(nums)-1, target)

# define recursion (nums, start, end, target)
def binarySearch(self, nums, start, end, target):
    
    # recursion exit
    if start > end:
        return -1
    
    mid = (start+end)//2
    # == target
    if nums[mid] == target:
        return mid
    # < target
    # start ..... mid+1 - [target] - end
    # meaning on the 'Right', then make start -> 'mid+1'
    if nums[mid] < target:
        return self.binarySearch(nums, mid+1, end, target)
        
    # > target
    # start - [target] - mid-1 ..... end
    # meaning on the 'Left', then make 'mid-1' <- end 
    return self.binarySearch(nums, start, mid-1, target)
        
```

## 模板FAQ:

#### Q1. 為什麼用start + 1 &lt; end? 

為了避免dead lock。這樣做的好處是讓範圍縮小至\[start,end\]讓start,end在不同的index，而不是可能的\[target\]無限循環。ex: \[1,1\] 就永遠跳不出去。

## Exponential Backoff 倍增法

使用到Exponential backoff的場景 \(1\) ArrayList in Java \(2\) 網路重試

## 練習

* [ ] First Bad Version
* [ ] Search Insert Position
* [ ] Find Minimum in Rotated Sorted Array
* [ ] Find Minimum in Rotated Sorted Array II
* [ ] Search In Rotated Sorted Array
* [ ] Search In Rotated Sorted Array II
* [ ] Search In 2D Matrix
* [ ] Search In 2D Matrix II



