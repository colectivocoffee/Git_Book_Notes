# \[Medium\] Find All Anagram in a String

Find All Anagram in a String  
Given a string **s** and a **non-empty** string **p**, find all the start indices of **p**'s anagrams in **s**.  
Strings consists of lowercase English letters only and the length of both strings **s** and **p** will not be larger than 20,100.  
The order of output does not matter.

#### Example

```text
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

## Thought Process

### Sliding Window\(-&gt;/-&gt;\): O\(n\)/O\(n\)

![](../.gitbook/assets/find_all_anagram.jpg)

## Code

```python
def findAnagrams(self, s: str, p: str) -> List[int]:
    
    #init window & need
    window = {}
    need = {}
    for char in p:
        need[char] = need.get(char,0) + 1
    
    #init pointers and results
    match = 0
    left, right = 0,0
    result = []
    
    while right < len(s):
        
        #1-1 move right pointer
        #1-2 update window data
        char = s[right]
        window[char] = window.get(char,0)+1
        if char in need:
            match += 1
        right += 1
                
        #2 shrink window
        while right-left >= len(p):
            
            #3-1 move left pointer
            #3-2 update window dat
            del_char = s[left]
            if del_char in need:
                match -= 1
            
            #4 update result 
            #### we get results by matching with len(need)
            if match == len(need):
                result.append(left)
            
            #update window
            window[del_char] -= 1      
            left += 1

    return result
                                
```



