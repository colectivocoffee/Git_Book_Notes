# \[Python\] Try/Except/Else/Finally

## How to Handle Exceptions/Errors? 

The exception is handled by **try,** **except** and **finally.**

* `try` – It checks the error in the block of code
* `except` – It handles the error
* `finally` – Optional, it's a clean-up action that must be executed all the time.

```python
try:
    list = [1,2,3,5,6,7]
    print(list[8])

# 下面只會三選一
except IndexError as e:
    print(e)
except:                       # 如果不是indexError，就會執行這個
    print('Some other error')
else:
    print('content is not here')
    
# 不管怎樣都會執行
print('continuing to this')    # except後，還是會繼續執行

# 這也是不管怎樣都會執行
finally: 
    # optional
    # clean up action that must be excecuted all the time. 
    print('always print the finally block')    


>>> IndexError, list index out of range 
>>> continuing to this
>>> always print the finally block 
```

### Raise an Error

> 用`if` 和 `raise` 的組合來產生Error。  
> （如果在class-class之間，通常會需要try/except來捕獲這個Error，否則會一直向上層傳遞。

```python
num = 'python'

if not type(num) is int:
    raise TypeError('Only integers are allowed here')

# >>> TypeError: Only integers are allowed here
```

### Types of Error in Python

| Error Type | How to trigger it? |
| :--- | :--- |
| AsserationError | It occurs when an assert statement fails |
| AttributeError | It occurs when attribute assignment fails |
| FloatingPointError | It occurs when the floating-point operation fails |
| MemoryError | It occurs when the operation is out of memory |
| IndexError | It occurs when the order is out of range |
| NotImplementedError | It occurs because of abstract methods |
| NameError | When the variable is not found in the local or global scope |
| KeyError | It occurs when the key is found in the dictionary |
| ImportError | It occurs when the imported module is not present |
| ZeroDivisorError | It occurs when the second operand is 0 |
| GeneratorExit | It occurs when the generator’s close\(\) is |
| OverFlowError | It occurs when the result of an arithmetic operation is too large |
| IndentationError | It occurs when the indentation is not correct |
| EOFError | It occurs when the input\(\) function end in the file condition |
| SyntaxError | It occurs when a syntax error is raised |
| TabError | It occurs when inconsistent space or tabs |
| ValueError | It occurs when the function gets a correct argument and an incorrect value |
| TypeError | It occurs when the function or operation is incorrect |
| SystemError | It occurs when the interpreter detects an internal error |

**Reference**  
[https://pythonguides.com/python-exceptions-handling/](https://pythonguides.com/python-exceptions-handling/)

