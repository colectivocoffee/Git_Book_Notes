# \[Medium\] Simplify Path

\*\*\*\*[**Simplify Path**](https://leetcode.com/problems/simplify-path/) \(1025/2034\)  
Given an **absolute path** for a file \(Unix-style\), simplify it. Or in other words, convert it to the **canonical path**.

In a UNIX-style file system, a period `'.'` refers to the current directory. Furthermore, a double period `'..'` moves the directory up a level.

Note that the returned canonical path must always begin with a slash `'/'`, and there must be only a single slash `'/'` between two directory names. The last directory name \(if it exists\) **must not** end with a trailing `'/'`. Also, the canonical path must be the **shortest** string representing the absolute path.

```python
Input: path = "/a/./b/../../c/"
Output: "/c"
```

## Code

### 1. Stack: O\(n\)/O\(n\)

Time Complexity O\(n\)  
Space Complexity **O\(n\)**: Actually it is 2N, since we have 1 stack and 1 path array to save path.split\('/'\)

這題其實就是要我們根據Unix系統的directory命令，輸出乾淨版的path。例如：

```text
/a/b/c/.././././//d      ->   /a/b/d
```

其中，`..`的功能是往上移一層；`.`的功能是待在原地；多餘的`//`可以被簡化成`/`。我們的目的就是把原來冗長的path簡化。而stack就可以幫助我們`往回看`。

等簡化完後，所有剩下留在stack裡的元素，就可以重新`''.join(stack)`，把他們re-construct the path again。

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

