# \[Medium\] Word Break

[Word Break](https://leetcode.com/problems/word-break/)

> Given a **non-empty** string _s_ and a dictionary _wordDict_ containing a list of **non-empty** words, determine if _s_ can be segmented into a space-separated sequence of one or more dictionary words.
>
> **Note:**
>
> * The same word in the dictionary may be reused multiple times in the segmentation.
> * You may assume the dictionary does not contain duplicate words.

#### Example:

```text
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```

```text
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```

## Thought Process

根據題意，"determine if _s_ can be segmented into..." 代表找DP的可行性，因此可以使用DP求解。

### \(1\) DP

### \(2\) DP + optimization

### \(3\) DFS

## Full Implementation

#### \(1\) DP

{% tabs %}
{% tab title="Python" %}
```python
def wordBreak(self, s: str, wordDict: List[str]) -> bool:
    
    if not s:
        return True
    
    if not wordDict:
        return False
    
    f = [False for i in range(len(s)+1)]
    
    # init state to ensure for loop start from 1
    f[0] = True
    for i in range(1, len(s)+1):
        for j in range(0, i): #<-- room to improve
            
            # 1: f[j] == True represents previous word is in wordDict, 
            # so then the next word should start from there. 
            # 2: s[j:i] represents the current word whether it is in wordDict.
            # If both conditions are met, then mark f[i] to True.
            if f[j] and s[j:i] in wordDict:
                f[i] = True
                break
            
     return f[-1]
```
{% endtab %}
{% endtabs %}

