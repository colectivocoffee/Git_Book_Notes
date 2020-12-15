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

### 1. Stack: O\(n\)/O\(n\)

Time Complexity: O\(n\) as linear search to put all numbers into the stack. When we do operator operations, it only requires O\(1\) for each of them. `n = len(tokens)`  
Space Complexity: O\(n\) for the stack that contains all numbers and no operators.

**思路：用Stack 後進先出的方式，把後面碰到operators的數字先處理，再一步一步把stack裡面的數字pop\(\)，直到最後剩下一個數字作結束。其中division truncate towards zero的要求，需要用到int\(a/b\)來滿足條件。**

1.Reverse Polish Notation 和 Infix Notation 不同點在於，Reverse Polish Notation 只需要按照順序把數字按下面的方式處理：  
`While there are operators remaining in the list, find the left-most operator. Apply it to the 2 numbers immediately before it, and replace all 3 tokens (the operator and 2 numbers) with the result.  
e.g. 2 3 4 + * --> 2 7 * --> 14`然而，Infix Notation是我們習慣的`“先乘除，後加減”`方法，這種方法用coding的方式處理起來比較複雜，暫時不討論。

2.在做`+-*/` operations時，pop\(\)哪一個數字先出來的順序很重要。  
`e.g. 7 - 5 ≠ 5 - 7`兩者結果不一樣。  
因此，pop\(\)第一次會先出來`right_num`，而後出來`left_num`。

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

### 2. Stack+lambda func: O\(n\)/O\(n\)

```python
def evalRPN(self, tokens: List[str]) -> int:
    
    stack = []
    operations = {
        '+' lambda: a, b: a + b, 
        '-' lambda: a, b: a - b,
        '*' lambda: a, b: a * b,
        '/' lambda: a, b: int(a/b)
    }
    
    for token in tokens:
        if token not in operations:
            stack.append(int(token))
        else:
            r_num = stack.pop()
            l_num = stack.pop()
            operation = operations[token]
            num = operation(l_num, r_num)
            stack.append(num)
    
    return stack[-1]
```

