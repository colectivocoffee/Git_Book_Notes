# \[Medium\] Minimum Remove to Make Valid Parentheses

[**Minimum Remove to Make Valid Parentheses**](https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/)  
Given a string s of `'('` , `')'` and lowercase English characters.  
Your task is to remove the minimum number of parentheses \( `'('` or `')'`, in any positions \) so that the resulting _parentheses string_ is valid and return **any** valid string.  
Formally, a _parentheses string_ is valid if and only if:

* It is the empty string, contains only lowercase characters, or
* It can be written as `AB` \(`A` concatenated with `B`\), where `A` and `B` are valid strings, or
* It can be written as `(A)`, where `A` is a valid string.

**Example 1:**

```text
Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.
```

**Example 2:**

```text
Input: s = "a)b(c)d"
Output: "ab(c)d"
```

**Example 3:**

```text
Input: s = "))(("
Output: ""
Explanation: An empty string is also valid.
```

## Code

### 1. Stack: O\(n\)/O\(n\)

Time Complexity: O\(n\)      \# of chars  
Space Complexity: O\(n\)    \# of chars on list

* **Convert string to list:** Because String is an immutable data structure in Python and it's much easier and memory-efficient to deal with a list for this task.
* Iterate through list
* **Add `'('` indices to the stack:** Keep track of indices with open parentheses in the stack. In other words, when we come across open parenthesis we add an index to the stack.
* **Pop index when we see `')'`:** When we come across close parenthesis we pop an element from the stack. If the stack is empty we replace current list element with an empty string.
* **Replace remaining '\(' from the stack:** After iteration, we replace all indices we have in the stack with empty strings, because we don't have close parentheses for them.
* Convert list back to string.

```python
def minRemoveToMakeValid(self, s: str) -> str:
    
    word = list(s)
    stack = []        # To save all '(' indices.
    
    for idx,char in enumerate(word):
        if char == '(':
            stack.append(idx)
        elif char == ')':
            if stack:
                stack.pop()
            else:
                # replace char into ''. To remove extra ')' 
                word[idx] = ''
        
        # for all the remaining alphabet, we do nothing and keep it out there.
    
    # if there's any '(' left in stack, meaning there's no ')' to pair with.
    # We then pop all of them from stack and replace it as ''.
    while stack:
        word[stack.pop()] = ''
    
    return ''.join(word)
```

