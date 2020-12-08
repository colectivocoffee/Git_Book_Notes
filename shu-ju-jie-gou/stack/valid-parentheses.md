# Valid Parentheses

\*\*\*\*[**Valid Parentheses**](https://leetcode.com/problems/valid-parentheses/)  
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

## Code

### 1. Stack + Dictionary: O\(n\)/O\(n\) 



```python
def isValid(self, s: str) -> bool:
    
    stack = []
    pair = {
        '(' : ')',
        '[' : ']',
        '{' : '}'    
    }
    
    for item in s:
        # if we find left bracket, then append it to the stack.
        if item in pair:
            stack.append(item)
        
        # if there's no matching left bracket in stack / right bracket doesn't match with item. 
        latest = stack.pop()
        elif len(stack) == 0 or pair[latest] != item:
            return False
        
    # stack should be empty in the end. If it is not empty, meaning 
    # there are unpaired bracket. Unmatched ones should return False.
    return len(stack) == 0
    
```

