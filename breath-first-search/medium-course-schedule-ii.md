# \[Medium\] Course Schedule II

[Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)  
There are a total of _n_ courses you have to take, labeled from `0` to `n-1`.  
Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite **pairs**, return the ordering of courses you should take to finish all courses.  
There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

## Thought Process

和[Course Schedule](https://app.gitbook.com/@iscolectivo/s/algonote/~/drafts/-MF2NLBiAnnK7tMZ0ltr/breath-first-search/medium-course-schedule)相同，都是要用**Topological Sort**。唯一不一樣的就是用`result = []`來取得course order。

這題同樣也是可以用DFS & BFS。\(DFS快很多\)

## Code

#### 1. BFS + Topological Sort \(188ms 19.92%\): O\(V+E\)

```python
def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
   # init
   indegree = [0 for _ in range(numCourses)]
   graph = [[] for _ in range(numCourses)]
   
   # construct relationship
   for ind,node in prerequisites:
      indegree[ind] += 1
      graph[node].append(ind)
   
   # add indegree == 0 nodes to queue
   queue = deque()
   for i in range(numCourses):
      if indegree[i] == 0:
         queue.append(i)
   
   # BFS
   result = []
   while len(queue) != 0:
      curr = queue.popleft()
      result.append(curr)
      for neighbor in graph[curr]:
         indegree[neighbor] -= 1        
         if indegree[neighbor] == 0:
            queue.append(neighbor)
   
   if len(result) == numCourses:
      return result
   else:
      return []
             
```

#### 2. DFS + Topological Sort \(104ms 84.81%\): O\(V+E\) 

```python
def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
    
    # init indegree & graph
    indegree = [0 for _ in range(numCourses)]
    graph = [[] for _ in range(numCourses)]
    
    # construct relationship
    for ind,node in prerequisites:
        indegree[ind] += 1
        graph[node].append(ind)
        
    # add all indegree == 0 nodes to stack
    stack = []
    for i in range(numCourses):
        if indegree[i] == 0:
            stack.append(i)
    
    # DFS
    result = []
    while len(stack) != 0:
        curr = stack.pop()
        # result is the order of courses to take
        result.append(curr)
        for neighbor in graph[curr]:
            # indegree - 1
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                stack.append(neighbor)
    
    if len(result) == numCourses:
        return result
    else:
        return []
```

