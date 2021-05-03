# 如何更好地Debug Recursion

```python
# 全局變量，紀錄目前Recursion層數
count = 0

# 根據總層數n，決定有多少個tabs
def printIndent(n):
   for i in range(len(n)):
      print('   ')  # a tab
```

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
```

```python
#### 加了debug helper function

# 全局變量，紀錄目前Recursion層數
count = 0                              # +

# 根據總層數n(aka count)，決定有多少個tabs
def printIndent(n):                    # +
   for i in range(len(n)):             # +
      print('   ')  # a tab


def dp(ring, i, key, j):
    printIndent(count+=1)              # +
    print('i = %d, j = %d\n', i, j)    # +
    
    if j == len(key):
        printIndent(count-=1)          # +
        print('return 0\n')            # +
        return 0
        
    result = sys.maxsize
    for k in range(len(key[j])):
        result = min(result, dp(ring, j, key, i + 1))
    
    printIndent(count-=1)              # +
    print('return %d\n', result)       # +
    return result


```

