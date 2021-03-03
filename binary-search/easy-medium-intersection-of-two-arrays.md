# \[Easy-Medium\] Intersection of Two Arrays

## [\[Easy\] Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)

### 1. Set: O\(N\)/O\(N\)

```python
def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

    set1 = set(nums1)
    set2 = set(nums2)

    return list(set1 & set2)
```

#### \[Medium\] Follow-Up: What if the array is sorted, can you do it in O\(N\) time and O\(1\) space?

### 1. Linear Search

```python
def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

    nums1.sort()  # assume the nums1 array is sorted
    nums2.sort()  #            nums2

    result = []
    if len(nums1) < 1 or len(nums2) < 1:
        return result

    i = 0
    j = 0
    while i < len(nums1) and j < len(nums2):       
        print(nums1[i], nums2[j])
        if nums1[i] == nums2[j]:
            if not result or result[-1] != nums1[i]:
                result.append(nums1[i])
            i += 1
            j += 1

        # move smaller element in nums2 forward   
        elif nums1[i] > nums2[j]:
            j += 1
        # move smaller element in nums1 forward
        else:
            i += 1

    return result
```

