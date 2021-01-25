# \[Easy\] Length of Last Word

\*\*\*\*[**Length of Last Word**](https://leetcode.com/problems/length-of-last-word/)   **\(929/2946\)**  
Given a string `s` consists of some words separated by spaces, return _the length of the last word in the string. If the last word does not exist, return_ `0`.

A **word** is a maximal substring consisting of non-space characters only.  


## Thought Process

### 1. String Functions, `.split(' ')` & `remove empty string`: O\(N\) / O\(N\)

Space Complexity: O\(N\)  .split\(' '\) returns a list of substrings, so we would need O\(N\) to store this list.  

有幾種特例要注意  
\(1\) `["a "]`：會變成`["a", ""]`，因此需要先Remove Empty String `""`  
\(2\) `[" "]`：會變成`[""]`，需要返回 0，因此也是要先Remove Empty String `""`

```python
def lengthOfLastWord(self, s: str) -> int:
    
    # split all words by ' ', will be a list.
    # e.g. ["Hello","World", ""]        
    split_words = s.split(' ')
    
    # remove all '' (empty string)
    # e.g. ["Hello","World"]
    words = [w for w in split_words if w]
    
    # s = [" "] -> []
    if not words:
        return 0
    
    return len(words[-1])
```

或是更簡潔的寫法

```python
def lengthOfLastWord(self, s: str) -> int:
    
    words = s.split()
    
    if words:
        return len(words[-1])
    return 0
```

### 2. while loop iteration: O\(N\)/O\(1\)

idx 從最後面開始往前走，如果碰到 `s[idx] != ' '`的話，就把答案+1，直到答案 &gt; 0 時，即代表已完成計算長度，就可返回答案。

```python
def lengthOfLastWord(self, s: str) -> int:
    
    length = 0
    idx = len(s)
    
    while idx > 0:
        idx -= 1
        if s[idx] != ' ':
            length += 1
        elif length > 0:
            return length 
        
    return length
```

