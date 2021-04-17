# \[Medium\] Merge Intervals

![](../../.gitbook/assets/image%20%2886%29.png)

## \[Medium\] Merge Intervals         \(7039/376\)

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

  If we can sort `intervals` in place, we do not need more than constant additional space, although the sorting itself takes O\(\log n\)O\(logn\) space. Otherwise, we must allocate linear space to store a copy of `intervals` and sort that.

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

