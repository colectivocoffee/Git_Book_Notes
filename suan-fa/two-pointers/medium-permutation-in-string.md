# \[Medium\] Permutation in String

[Permutation in String](https://leetcode.com/problems/permutation-in-string/)  
Given two strings **s1** and **s2**, write a function to return true if **s2** contains the permutation of **s1**. In other words, one of the first string's permutations is the **substring** of the second string.

#### Example

```text
Input: s1 = "ab" s2 = "eidbaooo"
Output: True
Explanation: s2 contains one permutation of s1 ("ba").
```

```text
Input:s1= "ab" s2 = "eidboaoo"
Output: False
```

## Thought Process

### Sliding Window: O\(n\)/O\(n\)

## Code

{% tabs %}
{% tab title="Python" %}
```python
def checkInclusion(self, s1: str, s2: str) -> bool:
    
    #init window & need
    window = {}
    need = {}
    for char in s1:
        need[char] = need.get(char,0)+1
    
    # init pointers and counter
    left,right = 0,0
    match = 0
    
    while right < len(s2):
        char = s[right]
        window[char] = window.get(char,0)+1
        # increment match (with condition of char in need) 
        if char in need:
            if window[char] == need[char]:
                match += 1
        # 1.move right pointer
        right+=1
        # 2.right-left to shrink window
        while right-left >= len(s1):
            # 4.update result
            if match == len(need):
                return True
            # 3.move left pointer
            del_char = s[left]
            if del_char in need:
                if window[del_char] == need[del_char]:
                    match -= 1
            # always forgot to shrink window -=1 !
            window[del_char] -= 1
            left+=1 
    return False
```
{% endtab %}
{% endtabs %}

