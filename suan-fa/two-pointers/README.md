# Two Pointers

## 思路

Two Pointers解法的精髓，是透過兩個指針控制一個區間\(window\)，保證區間滿足特定的條件。

### 1. 同向指針\( L-&gt;/R-&gt;\)

### 2. 相向指針\(L-&gt;/&lt;-R\)

相向指針模板  
\(1\) While Loop 

```python
L, R = 0, len(nums)-1

while L < R:
    
    # If statement is not required. 
    # Sometimes it will just do something without if/elif statement.
    if nums(L) nums(R) == target:
        L += 1
        R -= 1 
        return result or add result to the final list
    elif > target
    
    # check its condition to add 'if' statement if necessary. 
    L += 1
    R -= 1
```

\(2\) For Loop

```java
for (int i = 0, j = nums.size()-1; i < j; i++, j--)
```

## 題目

* [Two Sum](https://leetcode.com/problems/two-sum/)
* [Three Sum](https://leetcode.com/problems/3sum/)



* [ ] Two Sum
* [ ] Three Sum
* [ ] Valid Palindrome
* [ ] Container With Most Water
* [ ] Find K Closest Elements

