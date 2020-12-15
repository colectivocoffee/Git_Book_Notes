# \[Medium\] Evaluate Reverse Polish Notation

\*\*\*\*[**Evaluate Reverse Polish Notation**](https://leetcode.com/problems/evaluate-reverse-polish-notation/) **\(1316/499\)**  
Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).  
Valid operators are `+`, `-`, `*`, `/`. Each operand may be an integer or another expression.  
  
**Note:**  
1. ****Division between two integers should truncate toward zero.  
2. The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.

**Example 1:**

```text
Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2:**

```text
Input: ["4", "13", "5", "/", "+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

## Code

```python
def evalRPN(self, tokens: List[str]) -> int:
    
    if len(tokens) == 0:
        return 0
    
    stack = [] 
    
    for token in tokens:
        # we need int(token) to avoid python str multiplication error later on.
        if token not in '+-*/':
            stack.append(int(token))
            continue
        
        # always pop from the right(aka popright()).
        # here the sequence matters.
        right_num = token.pop()
        left_num = token.pop()
        
        if token == '+':
            num = left_num + right_num
        elif token == '-':
            num = left_num - right_num
        elif token == '*':
            num = left_num * right_num
        else: #division, token == '/' case
            num = int(left_num / right_num)   # 易錯點：需要注意division時rounding問題，
                                              # *** truncate towards zero 
                                              # int(a/b) will be useful 
                                              # e.g. -9/5 = -1, not -2 in this case.
                                              
        # append the calculated result back to stack for future use.
        stack.append(num)
    
    return stack[-1]
```

