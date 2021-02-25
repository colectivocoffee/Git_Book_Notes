# \[Easy\] Check if Array is Sorted and Rotated

## \[Easy\] Check if Array is Sorted and Rotated     \(125/24\)

Given an array `nums`, return `true` _if the array was originally sorted in non-decreasing order, then rotated **some** number of positions \(including zero\)_. Otherwise, return `false`.

There may be **duplicates** in the original array.

**Note:** An array `A` rotated by `x` positions results in an array `B` of the same length such that `A[i] == B[(i+x) % A.length]`, where `%` is the modulo operation.

```text
Ex1
Input: nums = [3,4,5,1,2]
Output: true
Explanation: [1,2,3,4,5] is the original sorted array.
You can rotate the array by x = 3 positions to begin on the the element of value 3: [3,4,5,1,2].

Ex2
Input: nums = [1,2,3]
Output: true
Explanation: [1,2,3] is the original sorted array.
You can rotate the array by x = 0 positions (i.e. no rotation) to make nums.

Ex3
Input: nums = [1,1,1]
Output: true
Explanation: [1,1,1] is the original sorted array.
You can rotate any number of positions to make nums.
```

### 1. Rotate Index + Index Slicing: O\(N\) / O\(N\)

Time Complexity: O\(N\)  Traverses two times would take 2N time  
Space Complexity: O\(N\) Index slicing would take another N space to build sorted\_nums

> 思路：`nums[i]`不應該比`nums[i+1]`大  
> 返回True的條件為sorted in non-decreasing order，意即後面的數`nums[i+1]`要比前面`nums[i]`大或相等，`nums[i] <= nums[i+1]`。  
> 反過來說，返回False的判斷條件即為 **`nums[i] > nums[i+1]`**。我們利用這個條件，第一次先找到rotate idx，第二次從sorted nums裡判斷是否還有`sorted_nums[i] > sorted_nums[i+1]`這種條件發生即可。

```python
def check(self, nums: List[int]) -> bool:
    
    # step1. Find where it gets rotated 
    rot_idx = 0
    while rot_idx < len(nums) - 1:
        if nums[rot_idx] > nums[rot_idx + 1]:   # nums[i]不應該比後面的數大
            break
        rot_idx += 1        
    # step2. 重建nums，排好序
    sorted_nums = nums[rot_idx+1:] + nums[:rot_idx+1]
    
    # step3. 如果重建後的nums還是有問題，即nums[i]比後面的數大，則False
    for i in range(len(sorted_nums)-1):
        if sorted_nums[i] > sorted_nums[i+1]:
            return False
    return True
```

