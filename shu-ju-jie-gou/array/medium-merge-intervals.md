# \[Medium\] Merge Intervals

![](../../.gitbook/assets/image%20%2886%29.png)

## \[Medium\] Merge Intervals         \(7039/376\)

```python
def merge(self, intervals: List[List[int]]) -> List[List[int]]:
    
    # sort intervals by each first element 
    intervals = sorted(intervals, key = lambda x:x[0])
    
    result = []
    
    # 1. completely overlapped 
    # 2. not overlapped
    # 3. partially overlapped
    for item in intervals:
        
        # not overlapped
        if not result or result[-1][1] < item[0]:
            result.append(item)
            
        # partialy overlapped
        # completely overlapped    
        # if (item[0] > # prev_item[0] 
        #     and item[1] > prev_item[1]) or
        else: 
            result[-1][1] = max(result[-1][1], item[1])           
    
    return result
```

