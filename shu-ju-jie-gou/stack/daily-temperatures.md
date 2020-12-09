# Daily Temperatures

\*\*\*\*[**Daily Temperatures**](https://leetcode.com/problems/daily-temperatures/)  
Given a list of daily temperatures `T`, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put `0` instead.

For example, given the list of temperatures `T = [73, 74, 75, 71, 69, 72, 76, 73]`, your output should be `[1, 1, 4, 2, 1, 1, 0, 0]`.

**Note:** The length of `temperatures` will be in the range `[1, 30000]`. Each temperature will be an integer in the range `[30, 100]`.  


## Code

### 1. Stack: O\(n\)/O\(n\)

```python
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        
        stack = []
        result = [0] * len(T)
        
        for idx, temp in enumerate(T):
            
            while stack and stack[-1][1] < temp:
                latest, val = stack.pop()
                
                # as the output described, 
                # it is returning the index instead of actual temperatures (how many days we should have to wait)
                result[latest] = idx - latest
            stack.append([idx, temp])
            
        return result    
```

