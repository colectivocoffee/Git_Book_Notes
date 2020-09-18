# BFS/DFS

![](../.gitbook/assets/bfs_and_dfs.jpeg)

## Stack and Queue \(棧和隊列\)

## Queue / BFS

Queue 用於BFS

#### How to Optimize BFS? 

如果在面試中問到“如何優化BFS”，那Bi-directional BFS 幾乎是標準答案了。

### 1. BFS w/o Level Order 模板 

```python
queue = dequeue()
visited = []

while len(queue)不為0:
    node = queue.pop()       # .popleft()
    for neighbor in 所有和node相鄰neighbor節點：
        if neighbor 未訪問過：
            queue.push(node) # .append()
            在visited紀錄此節點已被訪問
```

```python
from collections import deque    #double-ended queue 

queue = deque()
seen = set()

seen.add(start)                     #Init: 在seen&queue的初始位置
queue.append(start)
while len(queue):
    head = queue.popleft()          #取current head
    for neighbor in head.neighbors: #for遍歷每一個和head連接的neighbor
        if neighbor not in seen:    #  如果沒看過：
            seen.add(neighbor)      #     加入seen
            queue.append(neighbor)  #     加入queue
```

### 2. BFS w/ Level Order 模板

如果用BFS來求最短路徑的話，queue中第一層和第二層的節點會在一起無法區分。因此，在每一層遍歷開始前，我們需要增加一個n，來紀錄queue中的節點數量，然後一口氣處理完這一層的n個節點。

```python
depth = 0
while len(queue) 不為0 :
    depth += 1
    n = queue中元素的個數
    for 循環 n 次：
        node = queue.popleft()
        for neighbor in 所有和node相鄰的節點：
            if neighbor 未訪問過：
                queue.push(neighbor)
                在visited記錄此節點已被訪問 
```

## Stack / DFS

Stack 用於DFS。

通常DFS都會採用**Recursion**的方式去實現。如果一個題目可以選擇使用BFS or DFS的情況下，一定要優先使用BFS，因為BFS non Recursive較DFS容易實現很多。

Time Complexity: `O(答案個數 * 產生每個答案的時間)`

### 1. DFS Recursive 模板

在Matrix上的DFS版本

```python
rows = len(matrix)
cols = len(matrix[0])
visited = [ [False]*cols for _ in range(rows) ]

def dfs(self, matrix, x, y, visited):
    if 滿足遞歸條件:          # 遞歸出口
        return xxx
    if 超過matrix邊界 or visited[x][y]: # 確認邊界&visited條件
        return xxx 
        
    visited[x][y] = True    # 暫時標記此neighbor節點已訪問
    result = self.dfs(...)  # Recursion
    visited[x][y] = False   # 從neighbor節點Backtrack 
    
    return result
```

### 2. DFS Iterative 模板

```python
stack []
visited = []

while len(stack) != 0:
    curr = stack.pop()
    for neighbor in 所有和node相鄰neighbor節點：
        if neighbor 未被訪問過:
            stack.append(neighbor)
            在visited記錄此neighbor節點已被訪問
```

## Topological Sort

1.統計所有點的In-degree，並初始化Topological序列為空。  
2.將所有In-degree = 0的點，也就是沒有任何依賴的點，放到BFS Queue中。  
3.將Queue中所有的點一個一個放出來，放到Topological序列中，每次釋放出某個點curr的時候，就訪問`curr`的相鄰點`neighbor`（即所有A指向的點），並把這些點的`In-degree - 1`。  
4.如果發現某個點的 `In-degree - 1 == 0`，則放入Queue中。  
5.直到`len(Queue) == 0`時，算法結束。 

```python
1. indegree = [0 for _ in range(all nodes)]
   graph = [[] for _ in range(all nodes)]
   
   for ind,node in range(node-dependency):
      indegree[ind] ++
      graph[node].append(ind) # add dependency

2. for i in range(all nodes)
      if indegree[i] == 0:
         add to queue

3. Do BFS
   result = xxx
   while len(queue) != 0:
      curr = queue.popleft() # pop an element out from queue
      result.add             # add result
      # explore neighbor from graph
      for neighbor in graph[curr]:
         # indegree - 1
         indegree[neighbor] --
         4. if indegree[neighbor] == 0:
            add to queue
            
 5. Output result 
```

While the graph is not empty :  
    0. Check if the graph has no cycle  
    1. Find all nodes that have no arrows going into them \(In-degree = 0\)  O\(N+E\)  
    2. Delete it, output it until all the nodes are gone.



* [x] Clone Graph
* [x] Word Ladder
* [x] Word Ladder II
* [x] Number of Islands
* [x] Binary Tree Level Order Traversal
* [ ] Search Graph Nodes
* [x] Course Schedule
* [x] Course Schedule II
* [ ] Knight Shortest Path
* [ ] Zombie in Matrix
* [ ] Graph Valid Tree
* [ ] Binary Tree Serialization
* [ ] Build Post Office II
* [ ] Alien Dictionary

