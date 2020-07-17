# \[Hard\] Minimum Window Substring

[Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)  
Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O\(n\).

* If there is no such window in S that covers all characters in T, return the empty string `""`.
* If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

#### Example

```text
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

## Thought Process

### Sliding Window: O\(n\)/O\(n\)

![](../../.gitbook/assets/min_window_substring%20%281%29.jpg)

## Code

#### Sliding Window

{% tabs %}
{% tab title="Python" %}
```python
def minWindow(self, s: str, t: str) -> str:

        # init window & need
        window = {}
        need = {}
        for char in t:
            need[char] = need.get(char,0)+1
        
        # init remaining pointers and results
        match = 0
        start,end = 0,0
        min_len = float('inf') #+inf
        left,right = 0,0
        
        '''
         // 滑動窗口核心四要素：
        1-1、右指針Char
        1-1、更新窗口
        1-2、右指針右移 
            2、while根據題意收缩窗口
            3-1、左指針del_char 
            3-1、更新窗口&收缩del_char窗口
            3-2、左指針右移
        4、根據題意計算结果(right-left) < min_len
        '''
        while right < len(s):
            
            #1-1 right pointer
            char = s[right]
            window[char] = window.get(char,0)+1
            
            #1-1 update window/match data (char)
            if char in need:
                if window[char] == need[char]:
                    match += 1
            
            #1-2 move right pointer+1
            right += 1
            
            #2-1 shrink window
            while match == len(need):
                
                #3-1 left pointer
                del_char = s[left]
                
                #3-2 update window/match data (del_char)
                if del_char in need:
                    if window[del_char] == need[del_char]:
                        match -= 1 
                    window[del_char] -= 1
                
                #4 update result
                if right-left < min_len:
                    start = left
                    end = right
                    min_len = right-left
                
                #3-1 move left pointer+1            
                left += 1
        
        ## if not found ##
        if min_len == float('inf'):
            return ''

        return s[start:end]
```
{% endtab %}
{% endtabs %}

Backup version

```python
def minWindow(self, s: str, t: str) -> str:
        
        # window
        window = {}
        need = {}
        # add char to need, if not exist then add it to dictionary.
        for char in t:
            need[char] = need.get(char,0) + 1
        
        # init params
        left, right = 0, 0
        match = 0
        start, end = 0, 0
        min_window = float('inf')
        
        '''
         // 滑動窗口核心四要素：
        1、右指針右移 
            2、根據題意收缩窗口 
            3、左指針右移更新窗口 
        4、根據題意計算结果
        '''
        while right < len(s):
            
            #1.
            char = s[right]            
            right += 1
            
            #update match
            if char in need:
                window[char] = window.get(char,0)+1
                if window[char] == need[char]:
                    match += 1
#                print(need, window, match)
            #2. shrink window
            # only when match length is the same as len(t) then stop searching.
            while match == len(need):
                # update start,end,min_window for the result
                if right-left < min_window:
                    end = right
                    start = left
                    min_window = right - left
                
                #3-1. move left pointer
                del_char = s[left]
                left += 1
                print(need, left, right, del_char)
                #3-2. update data. 
                #skip the un needed chars
#                if need.get(del_char) != None:
                if del_char in need:
                    
                    if window[del_char] == need[del_char]:
                        match -= 1
                        
                    window[del_char] -= 1
                
                
        # edge case
        if min_window == float('inf'):
            return ''
        # 4. result 
        return s[start:end]
```

