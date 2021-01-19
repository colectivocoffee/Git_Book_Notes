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

```python
def longestValidParentheses(self, s: str) -> int:
    
    if not s:
        return 0
    
    # dp[i] records longest valid parentheses ending at s[i]
    # e.g.  ') ( ) ( ) )' 
    #    dp [0 0 2 0 2 4]
    #     i  0 1 2 3 4 5
    dp = [0] * len(s)
    result = 0
    
    # skip the first one regardless 
    # 因為我們只看 ')' 開頭
    for i in range(1, len(s)):
        if s[i] == ')': 
            if s[i - 1] == '(':
                dp[i] = dp[i - 2] + 2
        
            elif i-dp[i-1]-1 >= 0 and s[i-dp[i-1]-1] == '(':
                dp[i] = dp[i-1] + 2 + dp[i - dp[i-1] - 2] if dp[i-1] > 0 else 0
                
                # if dp[i - 1] > 0:
                #     dp[i] = dp[i-1] + 2 + dp[i - dp[i-1] - 2]
                # else:
                #     dp[i] = 0

        result = max(result, dp[i])
    return result

```

