# \[Medium\] Meeting Rooms II

[Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)  
Given an array of meeting time intervals consisting of start and end times `[[s1,e1],[s2,e2],...]` \(si &lt; ei\), find the minimum number of conference rooms required.

**Example 1:**

```text
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
```

**Example 2:**

```text
Input: [[7,10],[2,4]]
Output: 1
```

## Analysis & Code

### **1.Chronological Ordering: O\(nlogn\) / O\(n\)**

Time Complexity: `O(nlogn)` sorting two arrays need `nlogn` time.  
Space Complexity: `O(n)`  Extra two arrays N, start&end are needed.

```python
def minMeetingRooms(self, intervals: List[List[int]]) -> int:

    if len(intervals) == 0:
        return 0
    
    all_start = []
    all_end = []
    
    # create the list of all start & end
    for i in range(len(intervals)):
        start = intervals[i][0] 
        end = intervals[i][1]
        all_start.append(start)
        all_end.append(end)
    
    # sort them to be used for later intervals iteration
    all_start.sort()
    all_end.sort()
    
    rooms = 0  
    end_id = 0
    for i in range(len(intervals)):
        # ..[0-20]....
        # ...[10-30]..
        # if end time overlaps with start time, then we need an extra meeting room.
        if all_start[i] < all_end[end_id]:
            rooms += 1
        #  ..[0-20]..[30-40]..
        # if there's vacant time between start&end, 
        # meaning there's a meeting room available to be used, just update endtime.
        else:
            end_id += 1 
```

### 2.min Heap:    O\(NlogN\) / O\(N\)

```python
def minMeetingRooms(self, intervals: List[List[int]]) -> int:

    if len(intervals) == 0:
        return 0
        
    # sort all meetings by start time
    intervals.sort(key = lambda x: x[0])
    
    # construct heap
    all_ends = []
    
    # use heapq to get latest end time (already done by min heap)
    for meeting in intervals:
        
        curr_start_time = meeting[0]
        curr_end_time = meeting[1]
        if all_ends and curr_start_time >= all_ends[0]:
            # meaning two meetings can use same room (with >= all_ends time)
            # heapreplace == heappop + heappush
            heapq.heapreplace(all_ends, curr_end_time)
        else:
            # otherwise add a meeting room by current meeting end time
            heapq.heappush(all_ends, curr_end_time)
    
    # remaining end times are eqivalent to # of meeting rooms required.
    return len(all_ends)    
        
    
```

## 



