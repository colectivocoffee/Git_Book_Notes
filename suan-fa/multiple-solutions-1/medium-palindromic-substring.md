# \[Medium\] Palindromic Substring

Palindromic Substring  
Given a string, your task is to count how many palindromic substrings in this string.  
The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

#### Example 

```text
Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```

```text
Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

## Thought Process

### 1. Brute Force: O\(n^3\)/O\(1\)

最直觀的作法，就是先列舉每一個combination，也就是  
for left in range\(len\(s\)\)   
    for right in range\(left,len\(s\)\)  
接下來看這些pointer所指向的substring是否為palindrome，是的話+1。

```python
for left in range(len(s)):
    for right in range(left, len(s)):
        current_s = s[left:right+1] # +1 to make s[0:1]
        if check palindrome method (current_s)
            counter += 1
```

### 2. DP: O\(n^2\)/O\(1\)

由於我們發現到palindrome的特性：只有odd number/even number \(`i,i and i,i+1`\)的palindrome這兩種情況。也就是  
`odd number: 'a', 'bbb'` ...  
`even number: 'bb', 'dddd',`...  
我們可以用isPalindrome方法分開處理，並且**用兩個pointer left&right來看從此點開始往左往右，這個substring是否為回文串**。  
當\(1\) s\[left\] == s\[right\] 符合條件，再加上 \(2\) left &gt;= 0 \(3\) right &lt; len\(s\)時，就可以判定這是個回文串。判斷完後 &lt;--- left  right ---&gt;直到找不到為止。  
       `'[a]bc'   odd number palindrome  
    <--left  
       right-->`     

## Code

#### 1. Brute Force: O\(n^3\)/O\(1\)

```python
def countSubstrings(self, s: str) -> int:

    counter = 0    
    for left in range(len(s)):
        for right in range(left,len(s)):
            # always put +1 here to account for the slicing. 
            # otherwise it will be like s[left:right] = s[0:0] -> ???
            current_s = s[left:right+1]  
            if self.isPalindrome(current_s) == True:
                counter += 1
    return counter

def isPalindrome(self, s):
    # add reversed_s to check
    reversed_s = s[::-1]
    for i in range(len(s)):
        if s[i] != reversed_s[i]:
            return False
    return True
```

#### 2. DP: O\(n^2\)/O\(1\)

```python
def countSubstrings(self, s: str) -> int:

    result = 0    
    for i in range(len(s)):
        result += self.isPalin(s,i,i)   #add  odd number palin
        result += self.isPalin(s,i,i+1) #add even number palin
    return result 
    
    
def isPalin(self, s, left, right):
    counter = 0
    while left >= 0 and right < len(s) and s[left] == s[right]:
        counter += 1
        # move left/right pointer out of this while loop.
        # ex: 'abc'-'a', once we found 'a' here, we then -= 1 to finish this loop.
        left -= 1    
        right += 1
    return counter
        
```

