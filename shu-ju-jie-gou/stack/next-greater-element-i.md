# Next Greater Element I

### 496. Next Greater Element I

You are given two arrays **\(without duplicates\)** `nums1` and `nums2` where `nums1`’s elements are subset of `nums2`. Find all the next greater numbers for `nums1`'s elements in the corresponding places of `nums2`.

The Next Greater Number of a number **x** in `nums1` is the first greater number to its right in `nums2`. If it does not exist, output -1 for this number.

#### Example

```text
Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
Output: [-1,3,-1]
Explanation:
    For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
    For number 1 in the first array, the next greater number for it in the second array is 3.
    For number 2 in the first array, there is no next greater number for it in the second array, so output -1.
```

## Thought Process:

## Code

### 1. Dictionary: O\(m\*n\) / O\(m\), m = len\(nums1\) & n = len\(nums2\)

Runtime only beats 10.96%

1. 利用Dictionary建立nums1 element & nums2 index 之間的映射。這樣做的好處是，可以很快locate element in nums2，直接快速定位該數字在原數組的位置。
2. 從剛找到的element往右邊尋找，直到找到next greater element時即可。 

```python
def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:

    if not nums1 or not nums2:
            return []
        
    location = {}
    for item in nums1:
        if item in nums2:
            location[item] = nums2.index(item)
        else:
            location[item] = -1
#        print(location)
        
    result = [ -1 for _ in range(len(nums1))]
#        for start in location.values():
    for curr_num, start in location.items():
        for j in range(start,len(nums2)-1):
            next_item = nums2[j+1]
            if next_item > nums2[start]:
#                    nums1_id = nums1.index(nums2[curr])
                nums1_id = nums1.index(curr_num)
                result[nums1_id] = next_item
                break
    return result
```

### 2. Dictionary + Stack: 

```python
def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
```

