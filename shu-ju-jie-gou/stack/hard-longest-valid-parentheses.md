# \[Hard\] Longest Valid Parentheses

\*\*\*\*[**Longest Valid Parentheses**](https://leetcode.com/problems/longest-valid-parentheses/)    \(4355/163\)  
Given a string containing just the characters `'('` and `')'`, find the length of the longest valid \(well-formed\) parentheses substring.

## Thought Process

### 1. Stack: O\(n\)/O\(n\)

Time Complexity: O\(n\)    n is the length of str.   
Space Complexity: O\(n\)  n is the len of the str to put on the stack.

```python
def longestValidParentheses(self, s: str) -> int:
    
    if not s:
        return 0
    
    stack = [-1]
    result = 0
    
    for idx, char in enumerate(s):
        if char == '(':
            stack.append(idx)
        else:                       # if s[stack[-1]] == '(':
            stack.pop()
            if not stack:
                stack.append(idx)
            else:
                result = max(result, idx - stack[-1])
    
    return result
```

### 2. Brute Force: O\(n^3\)/O\(n\)

### 3. Dynamic Programming: O\(n\)/O\(n\)

