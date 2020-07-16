# \[Medium\] Longest Substring without Repeating Characters

[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)  
Given a string, find the length of the **longest substring** without repeating characters.

#### Example

```text
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```

## Thought Process

### 1. Two Pointers\(-&gt;/-&gt;\) + Hashset: O\(n^2\)/O\( $$\Sigma \ charset$$ \)

### 2. Sliding Window: O\(?\)/O\(1\)

```go
 // 滑动窗口核心点：
 1、右指针右移 
 2、根据题意收缩窗口 
 3、左指针右移更新窗口 
 4、根据题意计算结果
```

## Code

#### 1. Two Pointers \(-&gt;/-&gt;\) + Hashset: O\(n^2\)/O\( $$\Sigma \ charset$$ \)

{% tabs %}
{% tab title="Python" %}
```python
def lengthOfLongestSubstring(self, s: str) -> int:

    if not s or len(s)<=1:
        return len(s)
    
    max_len = 0
    for i in range(0, len(s)):
        char_set = set()
        curr_len = 0
        for j in range(i, len(s)):
            if s[j] in char_set:
                curr_len = j-i
                break
            else:
                curr_len += 1
                char_set.add(s[j])
        max_len = max(curr_len, max_len)
    return max_len
```
{% endtab %}
{% endtabs %}

#### 2. Sliding Window: O\(n\)/O\(dict size\)

```python
def lengthOfLongestSubstring(self, s: str) -> int:

    if not s or len(s) <= 1:
        return len(s)
    
    window = {}
    left, right = 0, 0
    max_len = 1
    
    #1.右指針右移
    #   2.根據題意收縮window
    #   3.左指針右移
    #4.根據題意計算結果
    while right < len(s):
        
        char = s[right]
        window[char] = window.get(char,0)+1
        right += 1
        
        while window[char] > 1:
            
            del_char = s[left]
            window[del_char] -= 1
            left += 1
            
        # here we get max_len from 'right-left'
        max_len = max(max_len, len(window))
    
    return max_len
```

