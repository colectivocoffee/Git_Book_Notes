# \[Medium\] Remove K Digits

Remove K Digits \(2876/124\)  
Given a non-negative integer num represented as a string, remove k digits from the number so that the new number is the smallest possible.  
**Note:**

* The length of num is less than 10002 and will be ≥ k.
* The given num does not contain any leading zero.

```text
Example1
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, 
and 2 to form the new number 1219 which is the smallest.

Example2
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. 
Note that the output must not contain leading zeroes.

Example3
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and 
it is left with nothing which is 0.
```

## Code

### 1. Greedy with Stack: O\(n\)/O\(n\)

Time Complexity: O\(n\) the time complexity of the main loop is bounded within 2N

```python
def removeKdigits(self, num: str, k: int) -> str:
    
    if not num:
            return '0'
        
    stack = []
    
    # check 1.stack  2.k  3. top stack > digit
    # in order to pop all largest digits from num. 
    for dgt in num:
        while stack and k > 0 and stack[-1] > dgt:
            stack.pop()
            k -= 1
        stack.append(dgt)
    
    # k is the requirement, so if k still have something out there, 
    # then we need to get stack cleared. 
    # - Trunk the remaining K digits at the end
    # - in the case k==0: return the entire list
    if k > 0:
        finalStack = stack[:-k]
    
    # handle case like 1) '0200' --> '200' (remove trailing zeros)
    # 2) num='9', k=1 -> '0' (nothing left should return 0) 
    return ''.join(finalStack).lstrip('0') or '0'
```

```python
def removeKdigits(self, num: str, k: int) -> str:
    
    if not num:
            return '0'
        
    stack = []
    del_ids = []
    
    for r_id in range(len(num)):
        while stack and k > 0 and num[stack[-1]] > num[r_id]:
            l_id = stack.pop()
            del_ids.append(l_id)
            k -= 1
        stack.append(r_id)
    
    result = []
    for i in range(len(num)):
        if i not in del_ids:
            result.append(i)
    
    if k > 0:
        result = result[:-k]
    
    return ''.join(result).lstrip('0') or '0'
```

