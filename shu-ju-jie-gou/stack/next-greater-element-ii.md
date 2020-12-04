# Next Greater Element II

[Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)  
Given a circular array \(the next element of the last element is the first element of the array\), print the Next Greater Number for every element. The Next Greater Number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, output -1 for this number.

**Example**

```text
Input: [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number; 
The second 1's next greater number needs to search circularly, which is also 2.
```

## Code

### 1. Brute Force: O\(n^2\)/O\(n\)

Time Complexity: O\(n^2\) The complete numsnums array of size nn is scanned for all the elements of numsnums in the worst case.  
Space Complexity: O\(n\) result array of size n is used.

由於題意說nums是個circular array，用circular來找next\_greater num，因此我們可以**double** nums，使之可以把nums最後一個element連到第一個element。

We are trying to find the next greater number for the `ith` number and `num[i] >= num[i+1]`，then we will go on to check num\[i+2\], num\[i+3\]... until we find the next\_greater num. 

### 2. Stack + Dictionary: O\(n\)/O\(n\) 

O\(n\) Time Complexity: Only two traversals of the nums array are done.   
O\(n\) Space Complexity: Stack size n is used, result array size n is used, mapping size n is used.  

{% hint style="info" %}
1. **Double** the nums array to make it circular.  `for i, num in enumerate(nums*2):`
2. We store **indices** of num as key, instead of num itself within the dictionary.  `mapping [idx] = num   # where idx = stack.pop()` 
3. **Stack** is used to compare next greater num. 
{% endhint %}

```python
def nextGreaterElements(self, nums: List[int]) -> List[int]:

        # test cases
        # nums = [5,4,3,2,1]
        # nums = [100,1,11,1,120,111,123,1,-1,-100]
        
        if not nums:
            return []
        
        mapping = {}
        stack = []
        result = []
                
        # duplicate nums to make it circular
        # NOTE: we store indexes instead of num in mapping, 
        # because there might be same num but different next_greater.
        for i, num in enumerate(nums*2): 
            while stack and nums[stack[-1]] < num:
#                print(mapping, stack, stack[-1])
                idx = stack.pop()
                mapping[idx] = num
            if i < len(nums):
                stack.append(i)
        
        # retrieve all next_greater and put it back to the result list.
        for i in range(len(nums)):
            next_greater = mapping.get(i, -1)
            result.append(next_greater)
        
        return result
```

