# Meeting Rooms II

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

1. Chronological Ordering: O\(n\) / O\(n\)

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
        # if end time overlaps with start time, then we need a extra meeting room.
        if all_start[i] < all_end[end_id]:
            rooms += 1
        #  ..[0-20]..[30-40]..
        # if there's vacant time between start&end, 
        # meaning there's a meeting room available to be used, just update endtime.
        else:
            end_id += 1 
```

