# \[Medium\] Random Pick with Weight

## [\[Medium\] Random Pick with Weight     \(1308/2904\)](https://leetcode.com/problems/random-pick-with-weight/)

You are given an array of positive integers `w` where `w[i]` describes the weight of `ith` index \(0-indexed\).  
We need to call the function `pickIndex()` which **randomly** returns an integer in the range `[0, w.length - 1]`. `pickIndex()` should return the integer proportional to its weight in the `w` array. For example, for `w = [1, 3]`, the probability of picking the index `0` is `1 / (1 + 3) = 0.25` \(i.e 25%\) while the probability of picking the index `1` is `3 / (1 + 3) = 0.75` \(i.e 75%\).  
More formally, the probability of picking index `i` is `w[i] / sum(w)`.

## Thought Process & Code

**Intuition**

Given a list of positive values, we are asked to _randomly_ pick up a value based on the weight of each value. To put it simple, the task is to do _**sampling with weight**_.

Let us look at a simple example. Given an input list of values `[1, 9]`, when we pick up a number out of it, the chance is that 9 times out of 10 we should pick the number `9` as the answer.

> In other words, the _**probability**_ that a number got picked is proportional to the value of the number, with regards to the total sum of all numbers.

To understand the problem better, let us imagine that there is a line in the space, we then project each number into the line according to its value, _i.e._ a large number would occupy a broader range on the line compared to a small number. For example, the range for the number `9` should be exactly nine times as the range for the number `1`. 踢球踢到9區間的概率是1區間的9倍。

![](../.gitbook/assets/image%20%28103%29.png)

![](../.gitbook/assets/image%20%28104%29.png)

> **prefix sum == cumulative sum**  
> the sum of all previous numbers in the sequence plus the number itself.  
>   
> **target**  
> To throw a ball on the line is to find an _offset_ to place the ball. Let us call this offset _target_.

### 1. Prefix Sum + Linear Search: O\(N\) & O\(N\) / O\(N\) & O\(1\)

Time Complexity: **O\(N\)** due to the init/construction of the prefix sums.  
Space Complexity: **O\(N\)**  init/constructor function is O\(N\)    
                                  **O\(1\)** pickIndex function is O\(1\)

{% tabs %}
{% tab title="Python" %}
```python
    def __init__(self, w: List[int]):
        
        self.prefix_sums = []
        current = 0
        for weight in w:
            current += weight
            self.prefix_sums.append(current)
        self.total = current
        
    '''
    This pickIndex function returns the result along with that number's probability.
    e.g. arr = [1,9], then picking 9 has the probability of 9/10.
    picking 1 has the probability of 1/10.  
    '''
    def pickIndex(self) -> int:
    
        # an offset to place the ball. 
        target = self.total * random.random()
        
        for i, current in enumerate(self.prefix_sums):
            if target < current:
                return i
```
{% endtab %}
{% endtabs %}

### 2. Prefix Sum + Binary Search: O\(N\) / O\(logN\) / O\(N\) & O\(1\)

{% tabs %}
{% tab title="Python" %}
```python
def __init__(self, w: List[int]):
    """
    [] prefix_sum: represents the list of the prefix sum so far.
    int total_sum: represents the total sum that 
                   has been added to prefix_sum so far.
    """
    
    self.prefix_sum = []
    current = 0
    for weight in w:
        current += weight
        self.prefix_sum.append(current)
    self.total_sum = current


def pickIndex(self) -> int:

    # an offset to place the ball. 
    target = self.total_sum * random.random()

    # run the binary search to search the target
    start, end = 0, len(self.prefix_sum)
    while start < end:
        mid = start + (end - start) // 2
        if self.prefix_sum[mid] < target:
            start = mid + 1
        else:
            end = mid

    return start
```
{% endtab %}
{% endtabs %}

