# \[Medium\] Task Scheduler

Task Scheduler  
Given a characters array `tasks`, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

However, there is a non-negative integer `n` that represents the cooldown period between two **same tasks** \(the same letter in the array\), that is that there must be at least `n` units of time between any two same tasks.

Return _the least number of units of times that the CPU will take to finish all the given tasks_.

## Code

這題`cooling time n`佔了很重要的因素。因此可以分為兩種不同狀態來討論：

#### \# cond1: n低於頻率最高task的數量，即cooling完後還可以再塞其他tasks。

![](../../.gitbook/assets/image%20%285%29.png)

#### \# cond2: n高於頻率最高task的數量，即還來不及cooling down，中間會有idle time來填。 

![](../../.gitbook/assets/image%20%284%29.png)

因此，第一種情況只要考慮len\(tasks\)即可。  
第二種情況，則是要找出一個規律，\(fmax - 1\) \* \(n + 1\) + nmax。詳細情況由上面可以推導出。  
最後，只要求`max(cond1, cond2)`即可。

### 1. Queue/Math: O\(n\_total\) / O\(1\)

`O(n_total)`  is a number of tasks to execute. All operations with it take constant time. 

```python

# tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2

def leastInterval(self, tasks: List[str], n: int) -> int:
    
    # cond1: all tasks can be placed without idle time
    cond1 = len(tasks)
    
    # cond2: need idle time, due to that there are more max tasks to be considered 
    # e.g.
    # Counter({'A': 6, 'B': 1, 'C': 1, 'D': 1, 'E': 1, 'F': 1, 'G': 1})
    # task_count_values = [6, 1, 1, 1, 1, 1, 1]
    task_count_values = list(collections.Counter(tasks).values())
    
    # find max count 
    highest_count = max(task_count_values)
    
    # how many max_count are there? 
    nmax = task_count_values.count(highest_count)
    
    # cond2
    cond2 = (highest_count - 1) * (n + 1) + nmax
    
    return max(cond1, cond2)
```

