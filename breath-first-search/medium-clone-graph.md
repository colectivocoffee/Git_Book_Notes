# \[Medium\] Clone Graph

[Clone Graph](https://leetcode.com/problems/clone-graph/)  
Given a reference of a node in a [**connected**](https://en.wikipedia.org/wiki/Connectivity_%28graph_theory%29#Connected_graph) undirected graph.  
Return a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) \(clone\) of the graph.

**Example**

```text
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```

## Thought Process

和BFS / DFS 模板幾乎一樣，但有些需要注意的地方。  
\(1\) **用dictionary來紀錄visited nodes：**並且key-value = node:nodeCopy  
\(2\) **make another deep copy of the node**：也就是`nodeCopy = Node(node.val)`。當然這也包含deepCopy of the neighbor nodes \(`neighborCopy = Node(neighbor.val)`\)。  
\(3\) **在visited建立curr和neighbor的關係**：也就是遍歷完這個neighbor node後，把它和原來的curr node 聯繫起來，因此我們要`visited[curr].neighbors.append(visited[neighbor])`

### 1. BFS

### 2. DFS \(Iterative/Recursive\)

## Code

#### 1. BFS

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""
def cloneGraph(self, node: 'Node') -> 'Node':
    if not node:
        return
    
    nodeCopy = Node(node.val)
    visited = {node: nodeCopy} # node:nodeCopy
    queue = deque([node])      # start from [node] 
    
    while len(queue) != 0:
        curr = queue.popleft()
        for neighbor in curr.neighbors:
            if neighbor not in visited:
                neighborCopy = Node(neighbor.val)
                queue.append(neighbor)           # neighbor not neighborCopy
                visited[neighbor] = neighborCopy    # 易錯點 
                                                    # node:nodeCopy     
            # regardless of the condition, 
            # add visited[neighbor] to visited curr's neighbors list
            visited[curr].neighbors.append(visited[neighbor])   
                     
    return nodeCopy
```

#### 2. DFS

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""
def cloneGraph(self, node: 'Node') -> 'Node':
    
    if not node:
        return
        
    nodeCopy = Node(node.val)
    visited = {node:nodeCopy}
    stack = []     # DFS uses stack
    
    while len(stack) != 0:
        curr = stack.pop()
        for neighbor in curr.neighbors:
            if neighbor not in visited:
                stack.append(neighbor)
                neighborCopy = Node(neighbor.val)
                visited[neighbor] = neighborCopy
            
            visited[curr].neighbors.append(visited[neighbor])
    
    return nodeCopy
    
```



