# \[Medium\] Remove All Adjacent Duplicates in a String II

Remove All Adjacent Duplicates in a String II  
ssss  
  
  
Given a string `s`, a _k_ _duplicate removal_ consists of choosing `k` adjacent and equal letters from `s` and removing them causing the left and the right side of the deleted substring to concatenate together.  
  
We repeatedly make `k` duplicate removals on `s` until we no longer can.  
Return the final string after all such duplicate removals have been made.  
It is guaranteed that the answer is unique.



### 1. Brute Force:

### 2. Stack: O\(n\)/O\(n\)

```python
def removeDuplicates(self, s: str, k: int) -> str:
    
    s = list(s)
    # init non-empty stack
    stack = [['#', 0]]
    
    for char in s:
        if stack or char != stack[-1][0]:
            stack.append([char,1])
        elif char == stack[-1][0]:
            # update count
            stack[-1][1] += 1
            if k == stack[-1][1]:
                stack.pop()
    
    # reconstruct string back
    return ''.join(char*times for char,times in stack)
    
    
```

### 3. Two Pointers: O\(n\)/O\(n\)

