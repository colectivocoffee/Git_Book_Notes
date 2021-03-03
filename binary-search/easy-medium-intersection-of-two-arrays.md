# \[Easy\] Intersection of Two Arrays / \[Easy\] Intersection II

## [\[Easy\] Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)

### 1. Set: O\(N\)/O\(N\)

```python
def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

    set1 = set(nums1)
    set2 = set(nums2)

    return list(set1 & set2)
```

#### \[Medium\] Follow-Up: What if the array is sorted, can you do it in O\(N\) time and O\(1\) space?

### 1. Linear Search Two Pointers: O\(N\) / O\(1\)

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

## \[Easy\] Intersection of Two Arrays II



**Follow up:**

* What if the given array is already sorted? How would you optimize your algorithm?
* What if nums1's size is small compared to nums2's size? Which algorithm is better?
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

### 1. Linear Search Two Pointers: O\(NlogN\) / O\(1\)

```python
def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:

    nums1.sort()
    nums2.sort()

    result = []

    i = 0
    j = 0
    while i < len(nums1) and j < len(nums2):
        if nums1[i] == nums2[j]:
            result.append(nums1[i])
            i += 1
            j += 1

        elif nums1[i] > nums2[j]:
            j += 1
        else:
            i += 1

    return result
```

### 2. Dictionary: O\(N\)

> 思路：用Dictionary計算counts，counts隨之增減。  
> 既然要找Intersection，可以用Dictionary/HashMap來紀錄nums1的每個element出現次數。  
> 每發現一個intersection，就count減一並加到result，  
> 直到小於0時就代表沒有，則可以跳過。

![](../.gitbook/assets/image%20%2826%29.png)

```python
def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:

    count = collections.Counter(nums1)
    result = []

    for num in nums2:
        if count[num] > 0:
            result.append(num)
            count[num] -= 1

    return result
```

