# \[Easy\] Valid Palindrome

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

### **2. Brute Force \(Two For Loops\)**

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

