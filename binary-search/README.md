# Binary Search

## Binary Search 模板

```python
start, end = 0, len(nums)-1

# use 'start + 1 < end' to prevent inifinte loop
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

