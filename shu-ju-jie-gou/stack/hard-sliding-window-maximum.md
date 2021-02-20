# \[Hard\] Sliding Window Maximum

## [\[Hard\] Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)    \(\)



```python
def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:

    dp = []

    for left in range(len(nums)-k+1):
        right = left + k        
        max_sliding_window = max(nums[left:right])
        dp.append(max_sliding_window)
    
    return dp
```

