# \[Medium\] Decode String

\*\*\*\*[**Decode String**](https://leetcode.com/problems/decode-string/) \(4244/204\)  
Given an encoded string, return its decoded string.  
The encoding rule is: `k[encoded_string]`, where the encoded\_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.  
You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.  
Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like `3a` or `2[4]`.

## Code

### 1. Stack \(or 2 stacks\): O\(maxK \* n\)/O\(m+n\)

maxK is the max value of k, where k is how many times we traverse this string to decode its pattern.  
where m is the number of letters \[a-z\], and n is the length of string s

{% hint style="info" %}
1. **用** **stack 儲存number&alphabets**
2. **用 `list[]`來記錄number&alphabets：**而非`string''`的原因，是因為string concatenation有可能需要用到O\(n^2\)的時間，而且每做一次concatenation就會需要新建一個string variable。如果用'`'.join(list[])`可以減少資源浪費。
3.  看到`closing bracket ]`結尾時組合字串：由於目前都是以list的方式儲存，我們必須要把它轉換成原來的string形式來用freq加成。
{% endhint %}

```python
'''
e.g.: 3[a2[c]] --> accaccacc

stack                                when char == ']'          when char == ']'
append()               pop()         (['a'],['2'])       pop() ([],['3'])
|             |       |             |  -> ['a','c','c']  |  |   -> ['a', 'c', 'c', 'a', 'c', 'c', 'a', 'c', 'c']
| ['a'],['2'] |       |             |                    |  |
|    [],['3'] |   =>  |    [],['3'] |                    |  |
---------------       ---------------                    ----

append ([], ['3'])
append (['a'], ['2'])
pop ['a'] ['2']
['a', 'c', 'c']
pop [] ['3']
['a', 'c', 'c', 'a', 'c', 'c', 'a', 'c', 'c']

'''
def decodeString(self, s: str) -> str:
        
    stack = []
    list_s = list(s)
    freq = []         # freq & alpha is a way of keeping track of 
    alpha = []        # how the latest string is formed
    
    for char in list_s:
        if char.isdigit():
            freq.append(char)
        elif char.isalpha():
            alpha.append(char)
        elif char == '[':
            # 當我們看到'['出現時，就把'['之前到前前一個'['中間，的所有字母數字都存到stack
            stack.append((alpha, freq))                  
            alpha , freq = [], []    # 必須要清空alpha & freq，重新開始計數，否則會無限循環
        elif char == ']':
            prev_alpha, prev_freq = stack.pop()          
            prev_freq_int = int(''.join(prev_freq)) 
            # the previous sequence (prev_alpha) that was popped + the current sequence (alpha) times n.
            alpha = prev_alpha + alpha * prev_freq_int  # build latest string based on freq
    
    return ''.join(alpha)
```

