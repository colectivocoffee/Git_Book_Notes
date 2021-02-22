# \[Hard\] Sliding Window Maximum

## [\[Hard\] Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)    \(5234/145\)

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.  
Return _the_ max sliding window.  


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

Step1. First while loop -- remove already seen element  
If an element in the deque and it is out of `i-(k-1)`, we discard them. We just need to poll from the head, as we are using a deque and elements are ordered as the sequence in the array

Step2. Second while loop -- remove smaller elements in queue  
Now only those elements within `[i-(k-1),i]` are in the deque. We then discard elements smaller than a\[i\] from the tail. This is because if a\[x\] &lt;a\[i\] and x&lt;i, then a\[x\] has no chance to be the "max" in `[i-(k-1),i]`, or any other subsequent window: a\[i\] would always be a better candidate.

Step3. at index `i >= k - 1` -- Add first element in queue to result  
As a result elements in the deque are ordered in both sequence in array and their value. At each step the head of the deque is the max element in \[i-\(k-1\),i\]



![](../../.gitbook/assets/sliding_window_max.jpg)



```python
import collections
def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:

    queue = collections.deque()
    result = []

    for i in range(len(nums)):
        
        # step1. poll/remove from first
        # that is out of range(already seen)
        # green '-'
        while queue and queue[0] < i - k + 1:
            queue.popleft()
        
        # step2. poll/remove smaller numbers
        # red 'x'
        while queue and nums[queue[-1]] < nums[i]:
            queue.pop()
        
        # now we are able to put elements into queue
        # append to last
        queue.append(i)
        
        # step3. add remaining first element to result
        # remaining part would be max num, then append them to the result
        if i >= k - 1:            
            result.append(nums[queue[0]])

    return result
```

