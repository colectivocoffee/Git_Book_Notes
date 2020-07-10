# Binary Search

如果題目說要O\(logn\)的複雜度，有很大機率是用Binary Search \(二分法\) 求解。

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

## 模板FAQ:

#### Q1. 為什麼用start + 1 &lt; end? 

為了避免dead lock。這樣做的好處是讓範圍縮小至\[start,end\]，而不是可能的\[target\]無限循環。

## Exponential Backoff 倍增法

使用到Exponential backoff的場景 \(1\) ArrayList in Java \(2\) 網路重試

