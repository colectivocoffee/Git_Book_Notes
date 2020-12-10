# \[Easy\] Valid Parentheses

\*\*\*\*[**Valid Parentheses**](https://leetcode.com/problems/valid-parentheses/)  
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

## Code

### 1. Stack + Dictionary: O\(n\)/O\(n\) 

Time Complexity: O\(n\) we traverse the entire string s only once. We also do stack push&pop at O\(1\) time.  
Space Complexity: O\(n\) worst case for all left bracket as input string s \(`e.g. (((((((`\) into stack, would be O\(n\).

{% hint style="info" %}
The **stack** data structure can come in handy here in representing this **recursive structure** of the problem. We can't really process this from the inside out because we don't have an idea about the overall structure. But, the stack can help us process this recursively i.e. from outside to inwards.
{% endhint %}

Stack在這裡可以用作Recursion來對待。 \(同時stack本身也是Recursion的一種實現方法\)  
  
Step1. 當我們看到left bracket\( `'(' , '[' , '{'`\)時，把他們壓入stack，等待未來right bracket出現時處理/抵銷。  
  
Step2-1. 當我們看到right bracket時，不做任何事，即continue。  
Step2-2. 當stack本身已經沒有left bracket \(即len\(stack\) == 0\) ，或是出現的item不符合dictionary上的對應value， 則直接返回False。  
  
Step3. 最後判斷是否stack還留有殘存的left bracket \(即len\(stack\) == 0\)，有的話則返回False，沒有的話則滿足所有條件，返回True。  


```python
def isValid(self, s: str) -> bool:
    
    stack = []
    pair = {
        '(' : ')',
        '[' : ']',
        '{' : '}'    
    }
    
    for item in s:
        # if we find left bracket, then append it to the stack.
        if item in pair:
            stack.append(item)
        
        # if there's no matching left bracket in stack / right bracket doesn't match with item. 
        latest = stack.pop()
        elif len(stack) == 0 or pair[latest] != item:
            return False
        
    # stack should be empty in the end. If it is not empty, meaning 
    # there are unpaired bracket. Unmatched ones should return False.
    return len(stack) == 0
    
```

