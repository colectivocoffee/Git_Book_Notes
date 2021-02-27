# \[Medium\] Sort Colors

## [\[Medium\] Sort Colors](https://leetcode.com/problems/sort-colors/)         \(4903/279\)

Given an array `nums` with `n` objects colored red, white, or blue, sort them [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) ****so that objects of the same color are adjacent, with the colors in the order red, white, and blue.  
We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

**Follow up:**

* Could you solve this problem without using the library's sort function?
* Could you come up with a one-pass algorithm using only `O(1)` constant space?

### 1. Sort Array: O\(NlogN\) / O\(1\)

直接用Sorted Function，加上 `nums[:] = xxx` 來直接替換原來的nums。

```python
def sortColors(self, nums: List[int]) -> None:
    """
    Do not return anything, modify nums in-place instead.
    """
    nums[:] = sorted(nums)
    return nums
```

### 2. Three Pointers \(1-pass\): O\(1\) / O\(1\)

由於Follow-Up Question限制我們不能用sort function，加上不能額外開新的數組，我們只能在原來的nums上動手腳。

> 思路：使用Three Pointers：`left, right, i`分別讓nums成為 `[000 (left) 1111 (i)(right) 22222]`  
> 依照題意，我們只有三種顏色，因此可以用這個辦法，把所有**0放到left pointer左邊**，**2放到right pointer右邊**，**其餘的都放在left&right pointer中間，並且由i來traverse所有的數字**。只要看到不符合規定的，就用`nums[i] swap with nums[left]` or `nums[right]`。  
> 其中要注意的一點，是在update`i`的時候，當`nums[i] == 2`時並不需要移動`i`，因為我們不知道被換過的`nums[i]`是屬於0,1,2其中一個。

```python
def sortColors(self, nums: List[int]) -> None:
    """
    Do not return anything, modify nums in-place instead.
    """
    if not nums:
        return 

    left, right = 0, len(nums)-1
    i = 0

    while i <= right:
        # 不用移i
        if nums[i] == 2:
            nums[i], nums[right] = nums[right], nums[i]
            right -= 1
        elif nums[i] == 0:
            nums[i], nums[left] = nums[left], nums[i]
            left += 1
            i += 1
        else:
            i += 1

    return nums
```

###   

