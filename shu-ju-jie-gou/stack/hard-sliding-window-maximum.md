# \[Hard\] Sliding Window Maximum

## [\[Hard\] Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)    \(\)



### 1. Brute Force: O\(N\*K\) / O\(1\)

Time Complexity: O\(N\*K\) k is the sliding window size, N is the number of elements in nums

```python
def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:

    dp = []

    for left in range(len(nums)-k+1):
        right = left + k        
        max_sliding_window = max(nums[left:right])
        dp.append(max_sliding_window)
    
    return dp
```

### 2. Queue/Deque: O\(N\) / O\(N\)

> 思路：用Queue來紀錄目前最大indices  
> \(1\) **Deque不需要保留任何比nums\[i\]還小的數字**  
> 由於我們一直要紀錄max\_num，而又只能看sliding window這幾個數字，因此我們可以想到用Queue來維持目前為止的最大值。只要Queue裡面有比`nums[i]`還小的數字，就代表可以踢掉那些Queue裡小數字。為什麼呢？因為Queue裡小數字不可能比`nums[i]`還大，就沒道理留著。  
>   
> 此時順勢往後移sliding window。然而，sliding window有個很麻煩的地方: 容易越界  
> \(2\) **Sliding Window不能越左右index邊界**  
> 我們需要確保最後的`queue[0]`在`i - k`的右邊界內，意即`queue[0] < i-k+1` 。ss

![](../../.gitbook/assets/sliding_window_max.jpg)



```python
import collections
def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:

    queue = collections.deque()
    result = []

    for i in range(len(nums)):
        
        # step1. poll/remove from first
        # green '-'
        while queue and queue[0] < i - k + 1:
            queue.popleft()
        
        # step2. poll/remove from last
        # red 'x'
        while queue and nums[queue[-1]] < nums[i]:
            queue.pop()
        
        # now we are able to put elements into queue
        # append to last
        queue.append(i)
        
        # remaining part would be max num, then append them to the result
        if i >= k - 1:            
            result.append(nums[queue[0]])

    return result
```

