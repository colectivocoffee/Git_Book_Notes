# \[Medium\] Merge Intervals / Meeting Rooms II / Insert Intervals

![](../../.gitbook/assets/image%20%2893%29.png)

## [\[Medium\] Merge Intervals](https://leetcode.com/problems/merge-intervals/)         \(7039/376\)

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

```python
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

### 1. Sorting + max\(a,b\):   O\(NlogN\) / O\(logN\)-O\(N\)

考慮到merge intervals的情況，就只有以下三種可能：

1. completely overlapped -- case 3, 5
2. partially overlapped -- case 2, 4
3. not overlapped --- case 1, 6

而我們的目標，就是希望能**把答案的每個intervals延長，越長越好**。

> 把result的每個interval延長，越長越好  
> --&gt; 先traverse每個interval/item  
> --&gt; \[case1\] **Not Overlapped**, 如果遇到新的interval，沒有和result裡任何一個有重合，就加到result裡。  
> --&gt; \[case2\] **Overlapped**, 把item跟result的最後一個比。  
>                   如果找到有重合，either是partially overlapped或是completely overlapped，都可以用  
>                   `max(原始,  新長度)`來把interval延長。  
>   
> 用`max(a,b)`的好處，是在於簡化if/elif/else這些對於每一種case的判定。由於正常情況下，我們需要對每一種情況，都做個別處理，而用max\(a,b\)就可以自動幫我們分類，不需要一個一個case來看。

* Time complexity : O\(nlogn\)

  Other than the `sort` invocation, we do a simple linear scan of the list, so the runtime is dominated by the O\(nlogn\) complexity of sorting.

* Space complexity : O\(logN\) \(or O\(n\)\)

  If we can sort `intervals` in place, we do not need more than constant additional space, although the sorting itself takes O\(logn\) space. Otherwise, we must allocate linear space to store a copy of `intervals` and sort that.

```python
def merge(self, intervals: List[List[int]]) -> List[List[int]]:
    
    # sort intervals by each first element 
    intervals = sorted(intervals, key = lambda x:x[0])
    
    result = []
    
    # 1. completely overlapped
    # 2. partially overlapped 
    # 3. not overlapped
    for item in intervals:
        
        # not overlapped
        if not result or result[-1][1] < item[0]:
            result.append(item)
            
        # partialy overlapped
        # completely overlapped    
        else: 
            result[-1][1] = max(result[-1][1], item[1])           
    
    return result
```

## \[Medium\] Meeting Rooms II

請參見（用Heap）

{% page-ref page="../heap/meeting-rooms-ii.md" %}

## [\[Medium\] Insert Intervals](https://leetcode.com/problems/insert-interval/)             \(2799/244\)

Given a set of _non-overlapping_ intervals, insert a new interval into the intervals \(merge if necessary\).  
You may assume that the intervals were initially sorted according to their start times.

```python
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

### 1. Sorting + max\(a,b\):    O\(NlogN\) / O\(N\)

```python
def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
    
    result = []
    
    intervals.append(newInterval)
    intervals = sorted(intervals, key = lambda x:x[0])
    
    for item in intervals:
        if not result or result[-1][1] < item[0]:
            result.append(item)  
        else:
            result[-1][1] = max(result[-1][1], item[1])
            
    return result
```

### 2. Modify first/last overlapping interval with Binary Search:    O\(N\) / O\(N\)

your interviewer would expect you to reason about the lower bound of your time complexity. In this problem, your lower bound is O\(N\) due to the possible shifting of every element after the new\_interval. So binary search is a good option, but kind of meaningless, unfortunately.

### 3. Greedy:   O\(N\)/O\(N\)

```python
def insert(self, intervals: 'List[Interval]', newInterval: 'Interval') -> 'List[Interval]':
    # init data
    new_start, new_end = newInterval
    idx, n = 0, len(intervals)
    output = []
    
    # add all intervals starting before newInterval
    while idx < n and new_start > intervals[idx][0]:
        output.append(intervals[idx])
        idx += 1
        
    # add newInterval
    # if there is no overlap, just add the interval
    if not output or output[-1][1] < new_start:
        output.append(newInterval)
    # if there is an overlap, merge with the last interval
    else:
        output[-1][1] = max(output[-1][1], new_end)
    
    # add next intervals, merge with newInterval if needed
    while idx < n:
        interval = intervals[idx]
        start, end = interval
        idx += 1
        # if there is no overlap, just add an interval
        if output[-1][1] < start:
            output.append(interval)
        # if there is an overlap, merge with the last interval
        else:
            output[-1][1] = max(output[-1][1], end)
    return output
```

![](../../.gitbook/assets/image%20%2890%29.png)

![Add to the output all the intervals starting before newInterval.](../../.gitbook/assets/image%20%2887%29.png)

![Add to the output newInterval. Merge it with the last added interval if newInterval starts before the last added interval.](../../.gitbook/assets/image%20%2886%29.png)

![Add the next intervals one by one. Merge with the last added interval if the current interval starts before the last added interval.](../../.gitbook/assets/image%20%2892%29.png)

