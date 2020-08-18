# \[Medium\] Course Schedule

[Course Schedule](https://leetcode.com/problems/course-schedule/)  
4101/182  
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses-1`.  
Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite **pairs**, is it possible for you to finish all courses?

## Code

#### 1. BFS + Topological Sort: O\(V+E\)

```python
# this problem can be solved with topological sort by detecting 
# whether it has cycles in it or not 
def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:

    # init indegree and graph
    # numCourses can be represented as node number
    # graph shows the connection between other nodes
    indegree = [ 0 for _ in range(numCourses)]
    graph = [[] for _ in range(numCourses)]

    # construct relationship between nodes
    # also calculate indegrees
    for ind,node in prerequisites:
        indegree[ind] += 1
        graph[node].append(ind)
    
    # topo sort step1:
    # put all indegree == 0 into queue
    queue = deque()
    for i in range(numCourses):
        if indegree[i] == 0:
            queue.append(i)
    
    # topo sort step2: do BFS
    count = 0
    while len(queue) != 0:
        curr = queue.popleft()
        # add count to make sure all the nodes can be reached by topo sort
        count += 1      
        for neighbor in graph[curr]:
            # topo sort step3: indegree -= 1
            # indegree - 1
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                queue.append(neighbor)
    
    # topo sort step4:
    # if final count matches with numCourses
    # then in the end all the nodes has indegree == 0
    if count < numCourses: 
        return False
    else:
        return True 
    
```









```text
        numCourses = 6
        prerequisites = [[2,1],[4,1],[3,2],[5,4]]
#        prerequisites = [[2,1],[4,1],[3,2],[5,4],[2,3]]
        
        graph = [[] for _ in range(numCourses)]
        indegree = [0 for _ in range(numCourses)]
        
        for take,need in prerequisites:
            if take not in graph[need]:
                indegree[take] += 1
                graph[need].append(take)
            
            #           0, 1, 2, 3, 4, 5
            #    graph [0, 0, 1, 1, 1, 1]
            # indegree [[], [2, 4], [3], [], [5], []]    
            print(indegree)
            print(graph)
                
        
        queue = deque()
        for i in range(numCourses):
            if indegree[i] == 0:
                queue.append(i)
                
        count = 0
        while len(queue) != 0:
            
            curr = queue.popleft()
            count += 1
            for neighbor in graph[curr]:
                indegree[neighbor] -= 1
                if indegree[neighbor] == 0:
                    queue.append(neighbor)
        
        if count < numCourses:
            return False
        else:
            return True
            
```



