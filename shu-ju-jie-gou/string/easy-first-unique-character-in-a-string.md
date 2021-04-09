# \[Easy\] First Unique Character in a String

## First Unique Character In a String

\*\*\*\*[**First Unique Character in a String**](https://leetcode.com/problems/first-unique-character-in-a-string/)    \(2575/129\)  
Given a string, find the first non-repeating character in it and return its index. If it doesn't exist, return -1.

## Thought Process

### 1. Ordered Dictionary: O\(N\) / O\(1\)   \(beats 53%\)

Time Complexity: O\(N\)    as we go thru the string s two times with length N.  
Space Complexity: O\(1\)   English alphabet only contains 26 chars.

```python
def firstUniqChar(self, s: str) -> int:
    
    # Use OrderedDict() -python   or LinkedHashMap() -java 
    # to preserve the order.
    chars = collections.OrderedDict()
    
    # fail safe add count to dict.
    for char in s:
        chars[char] = chars.get(char, 0) + 1
    
    # first occurance of the count == 1
    for char, count in chars.items():
        if count == 1:
            return s.index(char)
    return -1

```

### 2. collections.Counter\(\) + enumerate: O\(N\) / O\(1\)

在leetcode上Runtime更快的答案，但是s被掃過兩遍。\(beats 87%\)

```python
def firstUniqChar(self, s: str) -> int:
    
    chars = collections.Counter(s)
    
    for idx, char in enumerate(s):
        if chars[char] == 1:
            return idx
    return -1
```

## First Repeated Character in a String

解法跟上面相同，就是需要把`if chars[char] == 2`

