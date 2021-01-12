# \[Hard\] Basic Calculator I / \[Medium\] Basic Calculator II

\*\*\*\*[**Basic Calculator I**](https://leetcode.com/problems/basic-calculator/) **\(1890/148\)**  
Given a string `s` representing an expression, implement a basic calculator to evaluate it.

```text
Example 
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
```

#### Follow Up Qs:

1. Support a ternary operator like in Java.
2. A method to print the expression string that identically to the input, preserves all the formats like extra spaces. Simply store the original input is not allowed.

## Thought Process

在做這題之前，有幾個要點需要先想明白：

\(1\) Implication of precedence by parenthesis：即內括號要先處理，再算外括號  
\(2\) The rules of addition and substraction：正負號的關係，即 負負得正

```python
e.g. '3-(2+(9-4))'

num| sign | stack        |result
 3    1    [1]            0           '3-(2+(9-4))'
 0   -1    [1]            3           ' -(2+(9-4))'
 0    1    [1, -1]        3           '  (2+(9-4))'
 2    1    [1, -1]        3           '   2+(9-4))'
 0    1    [1, -1]        1           '    +(9-4))'
 0    1    [1, -1, -1]    1           '     (9-4))'
 9    1    [1, -1, -1]    1           '      9-4))'
 0   -1    [1, -1, -1]    -8          '       -4))'
 4   -1    [1, -1, -1]    -8          '        4))'
 0   -1    [1, -1]        -4          '         ))'
 0   -1    [1]            -4          '          )'
 0    1    [1]            -4  #final  
```

## Code

### 1. Stack:O\(n\)/O\(n\)

```python
def calculate(self, s: str) -> int:

    stack = [1]     # stack here is to preserve all inherited operators. e.g.-- -> + 
    sign = 1       # sign = 1 is represents '+', whereas -1 represents '-' 
    num = 0
    result = 0
    
    # add +'+' in order to handle last digits 
    for char in s + '+':
        if char == ' ':
            continue
        elif char.isdigit():
            # to preserve all digits that are 10 and above e.g. 11, 23, 545 ...
            num = 10*num + int(char)
        elif char in '+-':
            # calc result
            result += sign * num * stack[-1]
            sign = 1 if char == '+' else -1
            # reset
            num = 0
        elif char == '(':
            # 由於有可能會碰到 '-( ...' 的情況，需要把前面的正負都存起來作判斷用
            stack.append(sign * stack[-1])
            # reset
            sign = 1
        elif char == ')':
            # inner parenthesis已結束，可以calc result 
            result += sign * num * stack[-1]
            # reset num
            num = 0
            # throw away last sign
            stack.pop()
            
    return result
```

#### Sample Answer of Follow Up Q: to support ternary operator 

```python
# s = "(1+(4+5+2)-3)+(6+8)?23:-1"
def calculate(self, s: str) -> int:

    stack = []
    sign = 1
    num = 0
    result = 0     
    
    before_question_idx = s.index('?')
    
    for char in s[:before_question_idx] + '+':
        
        if char.isdigit():
            num = 10 * num + int(char)
            
        elif char == '+':
            result += sign * num
            sign = 1
            # reset
            num = 0
            
        elif char == '-':
            result += sign * num 
            sign = -1
            # reset
            num = 0
        
        elif char == '(':
            stack.append(result)     # (1in) result
            stack.append(sign)       # (2in) sign
            # reset
            sign = 1
            result = 0
            
        elif char == ')':
            result += sign * num

            result *= stack.pop()    # (1out) sign
            result += stack.pop()    # (2out) result
            # reset 
            num = 0    
    
    # ternery operator -> xxxx ? a : b       
    # e.g. "(1+(4+5+2)-3)+(6+8)?23:-1"
    # 取 s[s.index('?')+1 : s.index(':')] 可以得到first_choice
    # 取 s[s.index(':')+1 : ] 可以得到sec_choice
    # limitation: 如果有spaces and/or +-() 在choices裡面的話，就要做更複雜的判斷。
    # 即把前面的+-()判斷變成一個function，到這裡時再次調用。
    first_choice_idx = s.index(':')
    first_choice = s[before_question_idx+1:first_choice_idx]
    sec_choice = s[first_choice_idx+1:]
    
    if result == int(first_choice):
        return first_choice
    return sec_choice
```

## Basic Calculator II



