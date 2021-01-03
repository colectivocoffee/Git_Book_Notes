# \[Medium\] Remove All Adjacent Duplicates in a String II

Remove All Adjacent Duplicates in a String II  
ssss  
  
  
Given a string `s`, a _k_ _duplicate removal_ consists of choosing `k` adjacent and equal letters from `s` and removing them causing the left and the right side of the deleted substring to concatenate together.  
  
We repeatedly make `k` duplicate removals on `s` until we no longer can.  
Return the final string after all such duplicate removals have been made.  
It is guaranteed that the answer is unique.



### 1. Brute Force:

Time Complexity: O\(n^2 / k\)  we can scan the string no more than n/k times.  
Space Complexity: O\(1\)          current string is used, no additional ones are created.

![](../../.gitbook/assets/image%20%282%29.png)

```python
def removeDuplicates(self, s: str, k: int) -> str:

    curr_id = 0
    
    while curr_id < len(s):
    
        #### condition of duplicates ####
        ### e.g. s = 'deeedbbcccbdaa', k = 3
        # then 'eee' would satisfy the following conditions:
        # 1. s[i : i+k] 
        # 2. s[i] * k 
        if s[curr_id : curr_id+k] == s[curr_id] * k:
            s = s[:curr_id] + s[curr_id+k:]    # reconstruct the string
            curr_id = 0                        # reset the count
        else:
            curr_id += 1
    
    return s

```

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

