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

### \(2\) Two Pointers \(同向雙指針\)

**Time Complexity: O\(nlogn\)  
Space Complexity: O\(n\)**

由於只有唯一解，我們發現此題如果排序後，用兩個同向雙指針：pointers Left & Right，L往右移，R往左移，就可以找到target sum。  
1. 先把nums sorted，並且用`(key, value) pair`的方式儲存其值和原始index。  
2. 開始比較。當while L &lt; R 時一直比，直到兩個指針相交，結束比較。  
3. if `nums[L][1] + nums[R][1] == target`時，返回原來的index，即 `[nums[L][0], nums[R][0]]`。  
4. 如果 nums\[L\]\[1\] + nums\[R\]\[1\] `> target`，則要 R -= 1  
5. 如果 `< target`，則 L += 1

延伸題型：  
3 Sum  
4 Sum 

## Code

#### \(1\) Dictionary\(Hashmap\)

{% tabs %}
{% tab title="Python" %}
```python
def twoSum(self, nums, target):

    if not nums or len(nums) == 0:
        return [-1, -1]
    
    dictionary = {}
    
    for i in range(len(nums)):
        second_num = target - nums[i]
        #(Yes)meet the status where second_num can be found in dictionary
        # verified by 加法原理
        if second_num in dictionary:
            return [i, dictionary[second_num]]
            
        #(No) does not meet the status, then add it to dictionary
        dictionary[nums[i]] = i    
        
    return [-1, -1]
```
{% endtab %}
{% endtabs %}

#### \(2\) Two Pointers

{% tabs %}
{% tab title="Python" %}
```python
def twoSum(self, nums, target):
    
    if not nums or len(nums) == 0:
        return [-1, -1]
    
    # 用(key,value)pair 存其原始index，放在key上。
    # 並且排序。
    # ex: [2,3,2,5] --> [(0, 2), (2, 2), (1, 3), (3, 5)]
    nums = enumerate(nums)
    nums = sorted(nums, key=lambda x: x[1])
    
    L, R = 0, len(nums)-1
    while L < R:
          
          # 用nums[L][1]來取value
          total = nums[L][1]+nums[R][1]
          if total == target:
              # 用nums[L][0]來取原始index
              return [nums[L][0], nums[R][0]]
          elif total < target:
              L += 1
          else:
              R -= 1
      
      return [-1, -1]
                      
```
{% endtab %}
{% endtabs %}



