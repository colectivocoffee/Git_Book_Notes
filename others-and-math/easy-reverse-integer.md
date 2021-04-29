# \[Easy\] Reverse Integer

## \[Easy\] Reverse Integer

### 1. Pop & Push Digits: O\(logX\) / O\(1\)

Time Complexity: O\(logX\) There are roughly $$\log_{10}(x)$$digits in x.

```python
def reverse(self, x: int) -> int:
    
    result = 0
    
    while x != 0:
        result = result * 10 + x % 10
        x = x // 10
    
    if result < float('-inf') or result > float('inf'):
        return 0
    else:
        return result
```

### 2. \(Recom\) String Reversal: 

```python
def reverse(self, x: int) -> int:
    
    # check initial one
    if self.checkOverflow(x):
        return 0
    
    # check converted one
    strg = str(x)
    if x >= 0:
        reverse = strg[::-1]
    # negative num
    else:
        # get num and reverse it
        only_num = strg[1:][::-1]
        reverse = '-' + only_num
    if self.checkOverflow(int(reverse)):
        return 0
    return int(reverse)
    
    
def checkOverflow(self, num):
    
    if num >= 2**31-1 or num <= -2**31-1:
        return True
    return False
```

