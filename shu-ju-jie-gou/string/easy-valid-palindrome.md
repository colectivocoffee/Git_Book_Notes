# \[Easy\] Valid Palindrome/ \[Easy\] Valid Palindrome II/ \[Easy\] Palindrome Number

[Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)  
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Note:** For the purpose of this problem, we define empty string as valid palindrome.  


#### Example

```text
Input: "A man, a plan, a canal: Panama"
Output: true
```

## Thought Process

### 1. Two Pointers \(L-&gt;/&lt;-R\)

這題的two pointer on valid palindrome思維，可以用在很多不同的題型上。（用同向雙指針）  
唯一的難點就是在remove punctuation & spaces & lower case。這題的話可以用`isalnum() + lower()`來解決。

**Time Complexity: O\(n\)  
Space Complexity: O\(1\)**

* Time Complexity: O\(N\) where N is the length of the string. Each of two checks of whether some substring is a palindrome is O\(N\).
* Space Complexity: O\(1\) additional complexity. Only pointers were stored in memory.

### **2. Brute Force \(Two For Loops\)**

基本上就是先remove punctuation & spaces & lower case，然後再一個一個char比較。

**Time complexity: O\(n^2\)  
Space complexity: O\(n\)**

## Code

{% tabs %}
{% tab title="Python" %}
```python
def isPalindrome(self, s: str) -> bool:
    
    if not s or len(s) == 0:
        return True
        
    alnum = ''
    for char in s:
        if char.isalnum():
            alnum += char.lower()
         
    L,R = 0, len(alnum)-1   
    while L < R:
        if alnum[L] != alnum[R]:
            return False
    
        L += 1
        R -= 1
    
    return True
    
```
{% endtab %}
{% endtabs %}

## \[Easy\] Valid Palindrome II 

Given a non-empty string `s`, you may delete **at most** one character. Judge whether you can make it a palindrome.

### 1. Two Pointer \(-&gt;/&lt;-\): O\(N\) / O\(1\)

題目的難點在於，可以del at most one character，因此不同於Valid Palindrom I 的部分是，我們要判斷  
`1. 刪左char` or `2.刪右char`。刪一個還是成立，即True or False -&gt; True。  
然而，當兩個都是False時，代表刪左刪右都不符合規定，意即至少要刪2個以上的chars。 

```python
# Test case: 'cscaac' False
#            'xyxz'   True
#            'xyz'    False
def validPalindrome(self, s: str) -> bool:

    left, right = 0, len(s)-1

    while left <= right:
        if s[left] == s[right]:
            left += 1
            right-= 1
        else: 
            # first/second代表的是del1char的結果1(刪左char)/結果2(刪右char)
            # e.g. cscaac  first  -> c caac (del 's')
            #       1  4   second -> csca c (del 'a')
            first = s[:left] + s[left+1:]      
            second = s[:right] + s[right+1:]
            
            # 邏輯閘：True or False   -> True
            return first == first[::-1] or second == second[::-1]

    return True
```

更易讀的寫法。  
易忘點：\(1\)原則上還是需要用邏輯閘check 結果1\(刪左char\)/結果2\(刪右char\)  
\(2\) 在checkPalin給pointer時，要給 `(left, right - 1)`  & `(left + 1, right)`

```python
def validPalindrome(self, s: str) -> bool:

    left, right = 0, len(s)-1 
    while left < right: 
        if s[left] == s[right]:
            left  += 1
            right -= 1
        else:
            # 易忘點：用邏輯閘判斷 刪左char or 刪右char
            #                      True or False -> True
            return self.checkPalin(s, left, right - 1) or self.checkPalin(s, left + 1, right)
    return True    


def checkPalin(self, s, left, right):
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    return True
```

## \[Easy\] Palindrome Number \(3002/1695\)

Given an integer `x`, return `true` if `x` is palindrome integer.  
An integer is a **palindrome** when it reads the same backward as forward. For example, `121` is palindrome while `123` is not.

**Follow up:** Could you solve it without converting the integer to a string?

### 1. String Conversion: O\(N\) / O\(1\)

```python
def isPalindrome(self, x: int) -> bool:
    x = str(x)
    return x == x[::-1]
```

### 2. Division + Revert Second Half: O\(logN\) / O\(1\)

直接轉換成String是最容易想到的，但如果Follow Up Question問要直接處理Integer的話，需要把x切兩半，比較前後半是否相同，能組成palindrome。  
前半用 reverted = reverted \* 10 + x % 10，後半用 x //= 10 。

```python
def isPalindrome(self, x: int) -> bool:

    if x <= 0 or x % 10 == 0:
        return False

    reverted = 0
    while x > reverted:
        reverted = reverted * 10 + x % 10
        x //= 10

    return x == reverted or x == reverted // 10
```

