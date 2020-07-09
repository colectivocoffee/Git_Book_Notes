# Binary Search

## Binary Search 模板

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

