# \[Easy\] Two Sum

[Two Sum](https://leetcode.com/problems/two-sum/)  
Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.  
You may assume that each input would have _**exactly**_ one solution, and you may not use the _same_ element twice.

#### Example

```text
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## Thought Process

### Category: Array

經典的Coding題。最容易想到的就是暴力解，用for i & for j，雙重for loop來看 nums\[i\] + nums\[j\] == target，Time Complexity O\(n^2\)。但這有兩個問題：1. for loop會重複算同樣的nums\[i\] which is not allowed，2. O\(n^2\)太大。  


### \(1\) Dictionary\(Hashmap\)

**Time Complexity O\(n\)  
Space Complexity O\(n\)**  
  
由於題意說“have _**exactly**_ one solution, and you may not use the _same_ element twice."，我們可以知道利用加法原理 `x + y = sum` --&gt; `y = sum - x`來替換第二個變量。題意又說，既然同一個數字不能使用兩次，我們可以用dictionary來記錄第一次出現的數字的index \( `(key,value) = (nums[i], i)` \)，我們就可以用一次的for loop來判斷`target - nums[i]` 是否在dictionary裡，這樣我們在`target - nums[i]`出現時，就可以知道已符合條件即可返回。

```python
定義邊界條件 

dictionary = {} # (key, value) = (nums[i], i)

for i in len(nums):
    定義second_num為target - nums[i]
    
    用second_num判斷是否在dictionary裡，
        如果符合，則返回兩數之index，即 [i, dictionary[second_num]]
    
    不符合，則把dictionary[nums[i]] = i 加到dictionary裡
    
邊界條件2，如果沒找到，返回None
```

### \(2\) Two Pointers

## Code

#### \(1\) Dictionary\(Hashmap\)

{% tabs %}
{% tab title="Python" %}
```python
def twoSum(self, nums, target):

    if not nums or len(nums) == 0:
        return None
    
    dictionary = {}
    
    for i in range(len(nums)):
        second_num = target - nums[i]
        #(Yes)meet the status where second_num can be found in dictionary
        # verified by 加法原理
        if second_num in dictionary:
            return [i, dictionary[second_num]]
            
        #(No) does not meet the status, then add it to dictionary
        dictionary[nums[i]] = i    
        
    return None
```
{% endtab %}
{% endtabs %}

#### \(2\) Two Pointers

