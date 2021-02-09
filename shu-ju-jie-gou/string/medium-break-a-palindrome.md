# \[Medium\] Break a Palindrome

## [\[Medium\] Break a Palindrome](https://leetcode.com/problems/break-a-palindrome/)    \(257/278\)

Given a palindromic string of lowercase English letters `palindrome`, replace **exactly one** character with any lowercase English letter so that the resulting string is **not** a palindrome and that it is the **lexicographically smallest** one possible.

Return _the resulting string. If there is no way to replace a character to make it not a palindrome, return an **empty string**._

A string `a` is lexicographically smaller than a string `b` \(of the same length\) if in the first position where `a` and `b` differ, `a` has a character strictly smaller than the corresponding character in `b`. For example, `"abcc"` is lexicographically smaller than `"abcd"` because the first position they differ is at the fourth character, and `'c'` is smaller than `'d'`.

### 1. Replace Char: O\(N\) / O\(N\)

> 思路：題目給定的string本身就是palindrome，因此可以把它切成兩半，並且只看前半部。要break a palindrome的辦法，就是換char。而換char的情況有以下三種：  
> 解法就是分開返回\(return\)即可。  
> \(1\) **normal palindrome**  e.g. `'abccba'`  -&gt; 看到的第一個非'a' char換成 'a'  
> \(2\) **only one char**             e.g. `'a'`           -&gt; 直接返回 `''`  
> \(3\) **all a's**                           e.g. `'aaaaa'`   -&gt; 既然都是'a'，那就把最後char換成'b'

```python
def breakPalindrome(self, palindrome: str) -> str:

    # 1.normal
    # 2.one char
    # 3.all a's

    s = palindrome

    # normal
    for i in range(len(s)//2):
        if s[i] != 'a':
            return s[:i] + 'a' + s[i+1:]

    # all a's
    if len(s) > 1:
        return s[:-1] + 'b'
    # one char
    return ''
```

