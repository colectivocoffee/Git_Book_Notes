# \[Hard\] Find Median from Data Stream

## [\[Hard\] Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)           \(3983/74\)

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

* For example, for `arr = [2,3,4]`, the median is `3`.
* For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

* `MedianFinder()` initializes the `MedianFinder` object.
* `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
* `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.

### 1. statistics.median:   \(TLE\)

```python
class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.arr = []

    def addNum(self, num: int) -> None:
        self.arr.append(num)

    def findMedian(self) -> float:
        return statistics.median(self.arr)
```

### 2. general sort:  O\(NlogN\) / O\(N\)

```python
class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.arr = []

    def addNum(self, num: int) -> None:
        self.arr.append(num)

    def findMedian(self) -> float:
        self.arr.sort()
        
        length = len(self.arr) - 1
        
        # odd number of items
        if length % 2 == 0:
            return self.arr[length//2]
        # even number of items
        # [1,2,3,4] 
             ^ ^    
        # (len//2) & (len//2 + 1)
        return (self.arr[length//2] + self.arr[length//2+1])/2.0
```

