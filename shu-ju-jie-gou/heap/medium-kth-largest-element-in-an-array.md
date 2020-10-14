# \[Medium\] Kth Largest Element in an Array

[Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)  
\(4320/285\) Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.  
Note:  
You may assume k is always valid, 1 ≤ k ≤ array's length.

**Example 1:**

```text
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```

**Example 2:**

```text
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```

## Code

### 1. 使用 Heap: O\(n logk\) / O\(k\) 

Time Complexity: O\(n logk\)，adding an element in a size k heap is O\(logk\)  
Space Complexity: O\(k\) to store heap elements  
在Python的Heap Library裡，有一個`Heapq.nlargest()`的Function，

```python
def findKthLargest(self, nums: List[int], k: int) -> int:

    return heapq.nlargest(k,nums)[-1]
```

```python
def findKthLargest(self, nums: List[int], k: int) -> int:

    
```

