# Two Pointers

## 思路

Two Pointers解法的精髓，是透過兩個指針控制一個區間\(window\)，保證區間滿足特定的條件。

## 1. 同向指針\( L-&gt;/R-&gt;\): O\(n\)

### **\(1\) Sliding Window 模板**

{% tabs %}
{% tab title="Python" %}
```python
    [a b c] a b c b b
     d   |
(left)-> |
       char
     (right) ->   
def slidingWindow(self, s):
    
    window = {}
    left, right = 0, 0
    result = 0
    # 0.右指針開始往右走
    while right < len(s):
        # char
        char = s[right]
        window[char] add ...
        #1.右指針右移
        right += 1
        
            #2.收縮窗口
        while window[char] > 1:
            # d
            d = s[left]
            window remove ...
            # 3.左指針右移
            left += 1 
        
        #4.更新答案 
        result = ....
```
{% endtab %}
{% endtabs %}

* [ ] Minimum Window Substring
* [x] Longest Substring Without Repeating Characters
* [ ] Longest Substring with At Most K Distinct Characters
* [ ] Minimum Size Subarray Sum
* [ ] Remove Nth Node From End of List

### **\(2\) Fast & Slow Pointers**

* [ ] Find the Middle of Linked List
* [ ] Linked List Cycle
* [ ] Linked List Cycle II

### \(3\)同向指針 isPalindrome 模板

{% tabs %}
{% tab title="Python" %}
```python
# s can be already sliced string like s[start:end]
def isPalindrome(self, s):
    i, j = 0, len(s)-1
    while i < j:
        if s[i] != s[j]:
            return False
        i += 1
        j -= 1
    return True


def isPalindrome(self, s):    
    reversed_s = s[::-1] #reverse s
    for i in range(len(s)):
        if s[i] != reversed_s[i]:
            return False
    return True
```
{% endtab %}
{% endtabs %}

## 題目類型

1. Remove Duplicates
2. Sliding Window
3. Middle of Linked List
4. Two Difference
5. Circle in a Linked List

#### 

## 2. 相向指針\(L-&gt;/&lt;-R\): O\(n\)

#### 相向指針模板 \(1\) While Loop 

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

#### \(2\) For Loop

```java
for (int i = 0, j = nums.size()-1; i < j; i++, j--)
```

#### 相關題目

* [ ] \*\*\*Two Sum & Related 
* [ ] \*\*Partition xxx & Related
* [ ] \*Reverse xxx & Related
* [ ] Longest Palindromic Substring
* [ ] Find K Closest Elements

## 練習

* [Two Sum](https://leetcode.com/problems/two-sum/)
* [Three Sum](https://leetcode.com/problems/3sum/)
* [ ] Two Sum
* [ ] Three Sum
* [ ] Valid Palindrome
* [ ] Container With Most Water
* [ ] Find K Closest Elements

