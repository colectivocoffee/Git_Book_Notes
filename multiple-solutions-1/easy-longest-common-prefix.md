---
description: string
---

# \[Easy\] Longest Common Prefix

[**Longest Common Prefix**](https://leetcode.com/problems/longest-common-prefix/) \(3533/2075\)  
Write a function to find the longest common prefix string amongst an array of strings.  
If there is no common prefix, return an empty string `""`.

```text
Ex1:
Input: strs = ["flower","flow","flight"]
Output: "fl"

Ex2:
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

## Code

### 1. Compare Using `enumerate`: O\(S\) / O\(1\)

Time Complexity: O\(S\)   S is the sum of all chars in strs.  
Space Complexity: O\(1\) 1 shortest string is used.

```python
def longestCommonPrefix(self, strs: List[str]) -> str:
    if not strs:
        return ''
    
    # 冗長寫法，找shortest word    
    shortest = ''
    shortest_len = sys.maxsize
    for word in strs:
        if len(word) < shortest_len:
            shortest_len = len(word)
            shortest = word
            
    # .... the same as below
```

找最短的word，更簡潔的寫法。

```python
def longestCommonPrefix(self, strs: List[str]) -> str:
    
    # 如果不處理empty strs的話，則會出現下面ValueError
    # ValueError: min() arg is an empty sequence
    if not strs:
        return ''
    
    shortest = min(strs, key=len)

    for i, char in enumerate(shortest):
        for word in strs:
            if word[i] != char:
                return shortest[:i]
    
    return shortest
```

### 2. Vertical Scanning, Horizontal Scanning

