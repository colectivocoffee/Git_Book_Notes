# Daily Temperatures

\*\*\*\*[**Daily Temperatures**](https://leetcode.com/problems/daily-temperatures/)  
Given a list of daily temperatures `T`, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put `0` instead.

For example, given the list of temperatures `T = [73, 74, 75, 71, 69, 72, 76, 73]`, your output should be `[1, 1, 4, 2, 1, 1, 0, 0]`.

**Note:** The length of `temperatures` will be in the range `[1, 30000]`. Each temperature will be an integer in the range `[30, 100]`.  


## Code

### 1. Brute Force/Next Array: O\(n\*w\) / O\(n+w\)

w: \# of allowed values for T\[i\]，  
n+w: size of T and size of next array 

### 1. Stack: O\(n\)/O\(n\)

Time Complexity O\(n\): n is the length of T  
Space Complexity O\(n\): n is the size of stack

按題意要求，是要找"再隔多少天才會有更暖的天氣“，換句話說其實就是和current temp溫度差是正+的index差。 因此，我們不需要管溫度差是差幾度，只需要知道index差別是多少即可。  
  
有幾點要注意，一，需要先用enumerate\(\)來取得\(key,value\) pair。  
二，溫度差可以用index - latest來取得。  
  
以下是pseudo code：

```python
for key, value in enumerate(T):
    while stack && 比較溫度差:
        stack.pop()
        放到result[i] = index差
    stack.append([key, value])    
```



```python
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        
        stack = []
        result = [0] * len(T)
        
        for idx, temp in enumerate(T):
            
            while stack and stack[-1][1] < temp:
                latest, _ = stack.pop()
                
                # as the output described, 
                # it is returning the index instead of actual temperatures (how many days we should have to wait)
                result[latest] = idx - latest
            stack.append([idx, temp])
            
        return result    
```

