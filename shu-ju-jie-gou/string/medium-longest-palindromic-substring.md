# \[Medium\] Longest Palindromic Substring

## \[Medium\] [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) \(9615/646\)

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

## Thought Process

### 1. Brute Force with isPalindrome: O\(n^3\)/O\(1\)

由兩根指針`left & right`來確定current\_length的長度，並且以`s[left:right+1]`作為此長度下的substring。

那left & right的範圍要如何確定呢？由於right不可能比left還左邊，因此只需要從left起始位置開始出發。我們的for loop可以定義為：

```python
max_len = 0
result = ''
# 總共有n(n-1)/2個substring，因此需要O(n^2)時間
for left in range(n):
    for right in range(left, n):
	     # 判斷palindromic substring需要O(n)時間
       if s[left:right+1]比max_len還長 and s[left:right+1]是否為palindrome:
					 則可以更新result
					 更新max_len

# O(n^2) * O(n) = O(n^3)時間
```

另外，每次更新的時候都需要和`max_len`比較，如果比`max_len`長，才紀錄left&right的值。

### 2. DP + Memoization: O\(n^2\) / O\(n\)

* Build a 2D True/False DP Matrix: 如果判斷為True，便可以往下走。
* First loop from right to left, second loop from left to right。 意即start從右往左掃，並且end從左往右掃。用這樣的方式，來不斷地減短長度，看是否為palindrome。
* Set the is Palindrome check along with dp\[i\]\[j\] check to update result。
* return the result with s\[start:start+max\_len\]

To improve over the brute force solution, we first observe how we can avoid unnecessary re-computation while validating palindromes. Consider the case "ababa". If we already knew that "bab" is a palindrome, it is obvious that "ababa" must be a palindrome since the two left and right end letters are the same.

We define $$P(i,j)$$ as following:

$$P(i,j) = \begin{cases} \text{true,} &\quad\text{if the substring } S_i \dots S_j \text{ is a palindrome}\\ \text{false,} &\quad\text{otherwise.} \end{cases}$$ 

Therefore,

$$P(i, j) = ( P(i+1, j-1) \text{ and } S_i == S_j )$$ 

The base cases are:

 $$P(i, i) = true$$ & $$P(i, i+1) = ( S_i == S_{i+1} )$$ 

This yields a straightforward DP solution, in which we first initialize the one and two letters palindromes, and work our way up finding all three letters palindromes, and so on...

### 3. Expand Around Center: O\(N^2\) / O\(1\)

由於palindrome是左右對稱，意味著往左右延展到longest substring也要相同。但要考慮兩種palindrome的可能：  
\(1\) odd palindrome    e.g. abcdcba   
\(2\) even palindrome  e.g. abcddcba  
我們可以用`index i`由左往右掃，同時用left&right指針設定左右邊界，看最長到哪。

### 4. Manachers Algorithm: O\(n\)/O\(n\)

## Code

### 1. Brute Force with PalindromeCheck: O\(n^3\)/O\(1\)

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

優化max\_len判斷句的版本

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

### 2. DP + Memoization:

```python
def longestPalindrome(self, s: str) -> str:

    n = len(s)
    dp = [[False] * n for _ in range(n)]
    longest = ''

    for i in range(n):
        dp[i][i] = True
        longest = s[i]

    max_len = 0
    for start in range(n-1, -1, -1):
        for end in range(start+1, n):
            print(dp)
            print('-------')
            if s[start] == s[end]:
                if end - start == 1 or dp[start+1][end-1]:
                    dp[start][end] = True
                    if end - start + 1 > max_len:
                        max_len = end - start + 1
                        longest = s[start:end + 1]
    return longest
```

### 3. Expand Around Center: O\(N^2\) / O\(1\)

> 思路：  
> \(1\)由`index i`開始同時往左\(left pointer\)+往右\(right pointer\)延展，找到最長的palindromic substring。找到後，和之前儲存`max_len`的substring比較，如果新的比舊的長，那就把`max_len`的palindromic substring換成新的。  
> \(2\)承上，會有兩種palindrome的可能，**odd palindrome** & **even palindrome**，因此兩個都需要和max\_len比較。

```python
# index 'i' 一直往右移，每到一個i，就從i往左往右延伸看目前最長的palindromic substring，
# 即expand from center。
# 同時看odd/even palindrome，如果發現比max_len大，則紀錄max_left & max_right的值
e.g.  "abcdccda"
                      max
      "abcdccda"  len substring
index L^R
 odd -1 1          1  a
even   01          0
max_left/max_right 0 0   max_len -> 1
-----------------------
      "abcdccda"
       L^R
 odd   0 2         1  b
even    12         0  
max_left/max_right 1 1   max_len -> 1
-----------------------
      "abcdccda"
        L^R
 odd    1 3         1  c
even     23         0  
max_left/max_right 2 2   max_len -> 1
-----------------------
      "abcdccda"
        L ^ R
 odd    1cdc5       3  cdc
even      34        0  
max_left/max_right 2 4   max_len -> 3
-----------------------
      "abcdccda"
          L^R
 odd      3 5       1  
even     2dccd7     4  dccd
max_left/max_right 3 6   max_len -> 4
-----------------------
      "abcdccda"
           L^R
 odd       4 6      1  
even        56      0  dccd
max_left/max_right 3 6   max_len -> 4
-----------------------
      "abcdccda"
            L^R
 odd        5 7     1  
even         67     0  dccd
max_left/max_right 3 6   max_len -> 4
-----------------------
      "abcdccda"
             L^R
 odd         6 8    1  
even          78    0  dccd
max_left/max_right 3 6   max_len -> 4
-----------------------
1 0 dccd
3 6 =====max


```

```python
def longestPalindrome(self, s: str) -> str:

    max_left, max_right = 0, 0
    for i in range(len(s)):
        len1 = self.checkPalin(s, i, i)
        len2 = self.checkPalin(s, i, i + 1)
        max_len = max(len1, len2)
        if max_len > max_right - max_left:
       #      "abcdccda"   len substring
       #          L^R
       #odd       3 5       1  
       #even     2dccd7     4  dccd    
       #          |-4|
       #          |i         i - (4-1)//2   
       #           i-|       i + 4//2
       # 根據max_len & i，重建原來的left pointer & right pointer並保存下來
            max_left = i - (max_len - 1) // 2
            max_right = i + max_len // 2   
    return s[max_left : max_right+1]

def checkPalin(self, s, left, right):

    while left >= 0 and right < len(s) and s[left] == s[right]:
        left -= 1
        right += 1
    print(left, right, s[left:right+1])
    return right - left - 1
```

優化判斷長度的部分，直接返回substring。

```python
def longestPalindrome(self, s: str) -> str:

    longest = ''
    for i in range(len(s)):
        odd = self.checkPalin(s, i, i)
        even = self.checkPalin(s, i, i+1)
        longest = max(longest, odd, even, key=len)
    return longest

def checkPalin(self, s, left, right):

    while left >= 0 and right < len(s) and s[left] == s[right]:
        left -= 1
        right += 1
    # 易錯點：s[left+1 : right]
    # 為什麼範圍不是left:right+1呢？
    # 因為left:right+1會包含s[left] != s[right]的char，而我們並不想要。
    # 因此左右各往內縮一格,being inclusive,才是我們要的palindromic substring。
    return s[left+1:right]
    
    # 如圖下所示
e.g.  "abcdccda"
         L ^  R
even     2dccd7     4  dccd 
```

