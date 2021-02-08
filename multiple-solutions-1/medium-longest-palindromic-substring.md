# \[Medium\] Longest Palindromic Substring

[Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)  
Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

## Thought Process

### 1. Brute Force with isPalindrome: O\(n^3\)/O\(1\)

由兩根指針`left & right`來確定current\_length的長度，並且以`s[left:right+1]`作為此長度下的substring。

那left & right的範圍要如何確定呢？由於right不可能比left還左邊，因此只需要從left起始位置開始出發。我們的for loop可以定義為：

```python
max_len = 0
result = ''
for left in range(n):
    for right in range(left, n):
	     if s[left:right+1]比max_len還長 and s[left:right+1]是否為palindrome:
					 則可以更新result
					 更新max_len
```

### 2. DP + Memoization: O\(n^2\)/O\(n\)

* Build a 2D True/False DP Matrix: 如果判斷為True，便可以往下走。
* First loop from right to left, second loop from left to right。
* Set the is Palindrome check along with dp\[i\]\[j\] check to update result。
* return the result with s\[start:start+max\_len\]

### 3. Expand Around Center: O\(N^2\) / O\(1\)

由於palindrome是左右對稱，意味著往左右延展到longest substring也要相同。但要考慮兩種palindrome的可能：  
\(1\) odd palindrome    e.g. abcdcba   
\(2\) even palindrome  e.g. abcddcba  
我們可以用`index i`由左往右掃，同時用left&right指針設定左右邊界，看最長到哪。

### 3. Manachers Algorithm: O\(n\)/O\(n\)

## Code

#### 1. Brute Force with PalindromeCheck: O\(n^3\)/O\(1\)

沒有優化的版本

```python
def longestPalindrome(self, s: str) -> str:

    result = ''
    max_len = 0

    for left in range(len(s)):
        for right in range(left, len(s)):                
            if len(s[left:right+1]) > max_len:
                if self.checkPalin(s[left:right+1]):
                    max_len = len(s[left:right+1])
                    result = s[left:right+1]       
    return result

def checkPalin(self, s):
    return s == s[::-1]
```

小優化版本

{% tabs %}
{% tab title="Python" %}
```python
def longestPalindrome(self, s: str) -> str:

    if not s:
        return ''

    max_len = 0
    result = ''
    
    # range is always like this:
    #        [b a b a r]
    # left:   0         len(s)
    # right:(left)      len(s)
    #
    # c_len = |__|      (right-left+1)
    for left in range(len(s)):
        for right in range(left, len(s)):
        
            current_len = right - left + 1
            
            # 優化：如果len比max_len短，沒必要算下去。
            # 並且可以讓isPalindrome只增加max_len時的result
            if current_len <= max_len:
                continue
                
            # current s = s[left:right+1]          
            if self.isPalindrome(s[left:right+1]):
                max_len = current_len
                result = s[left:right+1]           
    
    return result
    
# utility method to check isPalindrome or not.
def isPalindrome(self, s):
    
    for i in range(len(s)):
        reversed_s = s[::-1]
        if s[i] == reversed_s[i]:
            return True   
    return False    

```
{% endtab %}
{% endtabs %}

### 3. Expand Around Center: O\(N^2\) / O\(1\)

```python
def longestPalindrome(self, s: str) -> str:

    max_left, max_right = 0, 0
    for i in range(len(s)):
        len1 = self.checkPalin(s, i, i)
        print(i, 'curr i / len1 / len2')
        len2 = self.checkPalin(s, i, i + 1)
        print('-----------------------')
        print(len1, len2, s[max_left:max_right+1])
        max_len = max(len1, len2)
        if max_len > max_right - max_left:
            max_left = i - (max_len - 1) // 2
            max_right = i + max_len // 2   
        print(max_left, max_right, '=====max')
    return s[max_left : max_right+1]

def checkPalin(self, s, left, right):

    while left >= 0 and right < len(s) and s[left] == s[right]:
        left -= 1
        right += 1
    print(left, right, s[left:right+1])
    return right - left - 1
```

```python
e.g. "abcdccda"
                  
      "abcdccda"  len substring
      L^R
even -1 1          1  a
 odd   01          0
max_left/max_right 0 0   max_len -> 1
-----------------------
      "abcdccda"
       L^R
even   0 2         1  a
 odd    12         0  
max_left/max_right 1 1   max_len -> 1
-----------------------
      "abcdccda"
        L^R
even    1 3         1  b
 odd     23         0  
max_left/max_right 1 1   max_len -> 1
-----------------------
      "abcdccda"
        L ^ R
even    1   5       1  c
 odd      34        0  
max_left/max_right 2 2   max_len -> 1
1 0 b
2 2 =====max
1 5 bcdcc
3 curr i / len1 / len2
3 4 dc
-----------------------
3 0 c
2 4 =====max
3 5 dcc
4 curr i / len1 / len2
2 7 cdccda
-----------------------
1 4 cdc
3 6 =====max
4 6 ccd
5 curr i / len1 / len2
5 6 cd
-----------------------
1 0 dccd
3 6 =====max
5 7 cda
6 curr i / len1 / len2
6 7 da
-----------------------
1 0 dccd
3 6 =====max
6 8 da
7 curr i / len1 / len2
7 8 a
-----------------------
1 0 dccd
3 6 =====max


```

