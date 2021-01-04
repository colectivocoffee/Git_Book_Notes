# \[Medium\] Remove All Adjacent Duplicates in a String II

Remove All Adjacent Duplicates in a String II  
ssss  
  
  
Given a string `s`, a _k_ _duplicate removal_ consists of choosing `k` adjacent and equal letters from `s` and removing them causing the left and the right side of the deleted substring to concatenate together.  
  
We repeatedly make `k` duplicate removals on `s` until we no longer can.  
Return the final string after all such duplicate removals have been made.  
It is guaranteed that the answer is unique.



### 1. Brute Force:

Time Complexity: O\(n^2 / k\)  we can scan the string no more than n/k times.  
Space Complexity: O\(1\)          current string is used, no additional ones are created.

![](../../.gitbook/assets/image%20%282%29.png)

```python
def removeDuplicates(self, s: str, k: int) -> str:

    curr_id = 0
    
    while curr_id < len(s):
    
        #### condition of duplicates ####
        ### e.g. s = 'deeedbbcccbdaa', k = 3
        # here 'eee' would satisfy the following conditions:
        # 1. s[i : i+k] 
        # 2. s[i] * k 
        if s[curr_id : curr_id+k] == s[curr_id] * k:
            s = s[:curr_id] + s[curr_id+k:]    # reconstruct the string
            curr_id = 0                        # reset the count
        else:
            curr_id += 1
    
    return s

```

### 2. Stack: O\(n\)/O\(n\)

![](../../.gitbook/assets/image%20%283%29.png)

由於需要持續往回看，目前的char總共出現幾次，因此如果把 `char & occur times`以`[char,1]`的方式，都存起來放在stack裡，等需要的時候看stack\[-1\]，就可以大幅度減少從頭開始loop的時間。  
  
Time Complexity: O\(n\)      where n is the length of the string.  
Space Complexity: O\(n\)    stack size.

{% hint style="info" %}
注意：  
1\) stack在剛開始的時候，需要看`stack[-1][0]`，因此必須先放個東西\['\#', 0\]在裡面。  
2\) 如果發現char == stack\[-1\]\[0\]時，則需要更新在stack上的count，並且還要同時看count是否已滿足條件k，如果滿足，則stack.pop\(\)。
{% endhint %}

```python
def removeDuplicates(self, s: str, k: int) -> str:
    
    s = list(s)
    # init non-empty stack
    stack = [['#', 0]]
    
    for char in s:
        if char == stack[-1][0]:
            # update count
            stack[-1][1] += 1
            # 已滿足條件k，直接去掉
            if stack[-1][1] == k:
                stack.pop()
        else:
            stack.append([char,1])
    
    # reconstruct string back, based on what's left on stack
    return ''.join(char*times for char,times in stack)
    
    
```

### 3. Two Pointers: O\(n\)/O\(n\)

略

#### Reference

[https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/discuss/392933/JavaC%2B%2BPython-Two-Pointers-and-Stack-Solution](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/discuss/392933/JavaC%2B%2BPython-Two-Pointers-and-Stack-Solution)

