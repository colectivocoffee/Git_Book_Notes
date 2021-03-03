# \[Easy\] Intersection of Two Arrays / \[Easy\] Intersection II

## [\[Easy\] Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)

Given two arrays, write a function to compute their intersection.

```text
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

### 1. Set: O\(N\)/O\(N\)

```python
def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

    set1 = set(nums1)
    set2 = set(nums2)

    return list(set1 & set2)
```

## \[Medium\] Follow-Up: What if the array is sorted, can you do it in O\(N\) time and O\(1\) space?

### 1. Sort, Two Pointers: O\(N\) / O\(1\)

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

Given two arrays, write a function to compute their intersection.  
\(和第一種的差別是，所有的重複intersection都要算在內\)

```text
Ex1
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```

**Follow up:**

* What if the given array is already sorted? How would you optimize your algorithm?
* What if nums1's size is small compared to nums2's size? Which algorithm is better?
* What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

### 1. Sort + Two Pointers: O\(NlogN\) / O\(1\)

You can recommend this method when the input is sorted, or when the output needs to be sorted.

![](../.gitbook/assets/image%20%2827%29.png)

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

### 2. Dictionary + Counter: O\(N+M\) / O\(min\(N,M\)\)

> 思路：**用Dictionary計算counts，counts隨之增減**。  
> 既然要找Intersection，可以用Dictionary/HashMap來紀錄nums1的每個element出現次數。  
> 每發現一個intersection，就count減一並加到result，  
> 直到小於0時就代表沒有，則可以跳過。

Time Complexity: O\(N+M\) where nn and mm are the lengths of the arrays  
Space Complexity: O\(min\(N,M\)\) We use hash map to store numbers \(and their counts\) from the smaller array.

![count&#x96A8;&#x4E4B;&#x589E;&#x6E1B;](../.gitbook/assets/image%20%2826%29.png)

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

## **Answer to Follow-up Questions**

1. What if the given array is already sorted? How would you optimize your algorithm?
   * We can use **Sol1 Sort+TwoPointers** dropping the sort of course. It will give us linear time and constant memory complexity.
2. What if _nums1's_ size is small compared to _nums2's_ size? Which algorithm is better?
   * **Sol2 Dictionary** is a good choice here as we use a hash map for the smaller array.
3. What if elements of _nums2_ are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?
   * If `nums1` fits into the memory, we can use **Sol2 Dicitonary** to collect counts for `nums1` into a hash map. Then, we can sequentially load and process `nums2`.
   * If neither of the arrays fit into the memory, we can apply some partial processing strategies:
     * Split the numeric range into subranges that fits into the memory. Modify **Sol2 Dictionary** to collect counts only within a given subrange, and call the method multiple times \(for each subrange\).
     * Use an external sort for both arrays. Modify **Sol1 Sort+TwoPointers** to load and process arrays sequentially.

