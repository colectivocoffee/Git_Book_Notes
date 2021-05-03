# 如何更好地Debug Recursion

## 用n tabs紀錄層數

```python
# 下面是需要Debug的程式
def dp(ring, i, key, j):

    if j == len(key):
        return 0
        
    result = sys.maxsize
    for k in range(len(key[j])):
        result = min(result, dp(ring, j, key, i + 1))
    
    return result

############## 分隔線 ###############
# 加了debug helper function








```

