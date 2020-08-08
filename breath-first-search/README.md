# BFS/DFS

## Stack and Queue \(棧和隊列\)

![](../.gitbook/assets/bfs_and_dfs.jpeg)

## Stack

Stack 用於DFS

## Queue

Queue 用於BFS



How to Optimize BFS? 

如果在面試中問到“如何優化BFS”，那Bi-directional BFS 幾乎是標準答案了。

### 1. BFS without Level Order 

```python
queue = dequeue()
visited = []

while len(queue)不為0:
    node = queue.pop()
    for node in 所有和node相鄰節點：
        if node 未訪問過：
            queue.push(node)
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

### 2. BFS with Level Order

如果用BFS來求最短路徑的話，queue中第一層和第二層的節點會在一起無法區分。因此，在每一層遍歷開始前，我們需要增加一個n，來紀錄queue中的節點數量，然後一口氣處理完這一層的n個節點。

```python
depth = 0
while len(queue) 不為0 :
    depth += 1
    n = queue中元素的個數
    for 循環 n 次：
        node = queue.popleft()
        for node in 所有和node相鄰的節點：
            if node 未訪問過：
                queue.push(node)
                在visited記錄此節點已被訪問 
```

