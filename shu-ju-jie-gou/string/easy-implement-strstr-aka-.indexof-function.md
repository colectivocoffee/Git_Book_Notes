# \[Easy\] implement strStr\(\) \(aka .indexOf\(\) function\)

\*\*\*\*[**Implement strStr\(\)**](https://leetcode.com/problems/implement-strstr/)    **2112/2203**  
Return the index of the first occurrence of needle in haystack, or `-1` if `needle` is not part of `haystack`.

```python
Example 1:
Input: haystack = "hello", needle = "ll"
Output: 2

Example 2:
Input: haystack = "aaaaa", needle = "bba"
Output: -1
Example 3:

Input: haystack = "", needle = ""
Output: 0
```

## **Code**

### **\#\#\# Clarification Qs to ask before coding:**

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

### 1. Substring, linear time slicing: O\(\(H-N\)\*N\) / O\(1\)

Time Complexity: O\(\(H-N\)\*N\) where `H` is `len(haystack)`, `N` is `len(needle)`。  
                               原始for loop 執行了 \(h - n\) 次，而 if statement 執行了n 次。總共就是 O\(\(h-n\)\*n\)  
Space Complexity: O\(1\)

```python
def strStr(self, haystack: str, needle: str) -> int:
        
        h, n = len(haystack), len(needle)
        
        # 為什麼要+1? 因為range的特性，最後一個char也要被掃到，就需要+1。
        # exec h - n times
        for i in range(h - n + 1):
            
            # exec n times
            if haystack[i : i + n] == needle:
                return i
        
        return -1
        
```

### 2. Two Pointers: O\(\(H-N\)\*N\) / O\(1\)

