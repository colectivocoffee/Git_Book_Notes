# \[Hard\] Basic Calculator I / \[Medium\] Basic Calculator II / 文字版

## \*\*\*\*[**Basic Calculator I**](https://leetcode.com/problems/basic-calculator/) **\(1890/148\)**

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

\(1\) Implication of **precedence by parenthesis**：即內括號要先處理，再算外括號  
\(2\) The rules of **addition and substraction**：正負號的關係，即 負負得正

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

### 1. Stack +/- :O\(n\)/O\(n\)

stack存的是之前的正負號。

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

### Without parenthesis version: 

普通版不需要額外用stack。stack是拿來存正負號的。

```python
def calculate(self, s: str) -> int:
           
    s = list(s)
    s.append('+')    # 額外'+'，來確保最後一個數字被加到result
    
    result = 0
    sign = 1    # 1 or -1
    num = 0
    
    for char in s:
        # space
        if char == ' ':
            continue
        # num, check if num is a 2or more digits ones
        elif char.isdigit():            
            num = num*10 + int(char)
        # +, -
        elif char in '+-':
            result += sign * num
            sign = 1 if char == '+' else -1
            num = 0
    return result
```

### Follow Up Q: to support ternary operator 

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

## [\[Medium\] Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)         \(2342/360\)

Given a string `s` which represents an expression, _evaluate this expression and return its value_.   
The integer division should truncate toward zero.

```python
Example 1:
Input: s = "3+2*2"
Output: 7

Example 2:
Input: s = " 3/2 "
Output: 1

Example 3:
Input: s = " 3+5 / 2 "
Output: 5
```

### 1. Stack nums: O\(n\) / O\(n\)

> 由於沒有括號，不需要擔心內括號的優先級。  
> 1.使用stack來存所有的數字。  
> 2.最後用`sum(stack)`來把所有數字加總

```python
def calculate(self, s: str) -> int:
    
    stack = []
    pre_sign = '+'
    num = 0
    
    for char in s+'+':
        
        if char == ' ':
            continue
        
        elif char.isdigit():
            num = 10*num + int(char)
            
        else:
            if pre_sign == '+':
                stack.append(num)

            elif pre_sign == '-':
                stack.append(-num)
            
            elif pre_sign == '*':
                stack.append( stack.pop() * num )

            elif pre_sign == '/':
                stack.append( math.trunc(stack.pop() / num) )
            
            num = 0
            pre_sign = char
        
    return sum(stack)
```

## Basic Calculator II - 文字版

```python
def calculate(input_s):

   num_dict = {
      'one': 1,
      'two': 2,
      'three': 3,
      'four': 4,
      'five': 5,
      'six': 6,
      'seven': 7,
      'eight': 8,
      'nine': 9,
      'ten': 10,
      'eleven': 11,
      'twelve': 12
   }

   operators = ['plus', 'minus', 'times']

   # s as list
   s = input_s.split(' ')
   s.append('plus')

   stack = []
   num = 0
   prev_sign = 'plus'

   for word in s:
      if word in num_dict:
         num = num_dict[word]

      elif word in operators:
         if prev_sign == 'plus':
            stack.append(num)
         elif prev_sign == 'minus':
            stack.append(-num)
         elif prev_sign == 'times':
            prev = stack.pop()
            stack.append(prev * num)
         elif prev_sign == 'divides'
            stack.append( math.trunc(stack.pop() / num) )
         num = 0
         prev_sign = word  # saves the operator for later use

   return sum(stack)


#################################
def test(inp, expected):
   result = calculate(inp)
   print('Ans {} | {}="{}"'.format(result == expected, inp, result))


print('Correct/Wrong, input, actual')
test('one plus one', 2)
test('two times two', 4)
```

### Follow Up Q: Add Parenthesis --&gt; Basic Calculator III

If we need to process parenthesis? What will you do? 

## [\[Hard\] Basic Calculator III](https://leetcode.com/problems/basic-calculator-iii/)            \(604/222\)

Implement a basic calculator to evaluate a simple expression string.  
The expression string contains only non-negative integers, `'+'`, `'-'`, `'*'`, `'/'` operators, and open `'('` and closing parentheses `')'`. The integer division should **truncate toward zero**.  
You may assume that the given expression is always valid. All intermediate results will be in the range of `[-231, 231 - 1]`.

```python
Example 1:
Input: s = "1+1"
Output: 2

Example 2:
Input: s = "6-4/2"
Output: 4

Example 3:
Input: s = "2*(5+5*2)/3+(6/2+8)"
Output: 21
```

**Follow up:** Could you solve the problem without using built-in library functions?

