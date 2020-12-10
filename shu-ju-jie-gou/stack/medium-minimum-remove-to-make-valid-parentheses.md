# \[Medium\] Minimum Remove to Make Valid Parentheses



## Code

```python
def minRemoveToMakeValid(self, s: str) -> str:
    
    words = list(s)
    stack = []        # To save all '(' indices.
    
    for idx,char in enumerate(words):
        if char == '(':
            stack.append(idx)
        elif char == ')':
            if stack:
                stack.pop()
            else:
                # replace char into ''. To remove extra ')' 
                words[idx] = ''
        
        # for all the remaining alphabet, we do nothing and keep it out there.
    
    # if there's any '(' left in stack, meaning there's no ')' to pair with.
    # We then pop all of them from stack and replace it as ''.
    while stack:
        words[stack.pop()] = ''
    
    return ''.join(words)
```

