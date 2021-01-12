# \[Medium\] Simplify Path

Simplify Path  
sss

```python
def simplifyPath(self, path: str) -> str:
    
    stack = []
    # divide all directories, including ''
    # e.g. /a/./b/../../c/"
    # -> ['', 'a', '.', 'b', '..', '..', 'c', ''] 
    path = path.split('/')
    
    for p_dir in path:
        # .. means to move a level up 
        if p_dir == '..':
            if stack:
                stack.pop() # move a level up
            # else do nothing. cuz there's nothing you can pop from stack
            
        # elif p_dir condition -> to skip empty string ''
        # therefore we get ['a','b','c'] and put it into the stack 
        elif p_dir and p_dir != '.':
            stack.append(p_dir)
    
    # re-construct the path again, it must start with '/'
    result = '/' + '/'.join(stack)
    
    return result
```

