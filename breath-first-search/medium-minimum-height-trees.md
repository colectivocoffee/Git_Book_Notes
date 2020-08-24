# \[Medium\] Minimum Height Trees

Minimum Height Trees  
For an undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees \(MHTs\). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

**Format**  
The graph contains `n` nodes which are labeled from `0` to `n - 1`. You will be given the number `n` and a list of undirected `edges` \(each edge is a pair of labels\).

You can assume that no duplicate edges will appear in `edges`. Since all edges are undirected, `[0, 1]` is the same as `[1, 0]` and thus will not appear together in `edges`.

**Example**

```text
Input: n = 4, edges = [[1, 0], [1, 2], [1, 3]]

        0
        |
        1
       / \
      2   3 

Output: [1]
```

```text
Input: n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]

     0  1  2
      \ | /
        3
        |
        4
        |
        5 

Output: [3, 4]
```

* According to the [definition of tree on Wikipedia](https://en.wikipedia.org/wiki/Tree_%28graph_theory%29): “a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”
* The **height of a rooted tree** is the **number of edges on the longest downward path between the root and a leaf**.

## Thought Process

由上面的Tree & height of a rooted tree定義，我們可以知道Minimum Height Tree是由“不斷地砍去leaves，直到最後只剩最短枝幹”所造，也就是從中點到枝葉最長距離 -- 其他次長的枝葉。因此我們可以用類似Topological Sort的方式求解。

唯一不同的是，Topological Sort是一層一曾向外推，而我們的解法是從最外的leaves開始一層一層向內推。`leaves = [i for i in range(n) if len(graph[i]) <= 1]`



## Code

> 思路：Reversed Topological Sort \(由外而內剪leaves\) + prev\_leaves, leaves 來存舊枝葉

```python
def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
    
    graph = [[] for _ in range(n)]
    
    # construct the graph
    # ex: edges = [[1, 0], [1, 2], [1, 3]]
    for node1,node2 in edges:
        graph[node1].append(node2)
        graph[node2].append(node1)
    #  node     0     1     2   3
    # graph = [[1],[0,2,3],[1],[1]]    
    
    # add all leaves
    leaves = []
    for i in range(n):
        if len(graph[i]) <= 1:  # <= 1 to include isolated nodes
            leaves.append(i)
    # leaves = [0,2,3] (from [1], i is the node index)
    
    while len(leaves) != 0:
        # to store leaves to be used/cutted in next iteration
        new_leaves = []
        for leaf in leaves:
            # if we cannot find leaf in graph, meaning it is the minimum height trees
            # then exit
            if not graph[leaf]:
                return leaves
    
            # remove leaf from both sides in the graph
            neighbor = graph[leaf].pop()
            graph[neighbor].remove(leaf)
            # all all leaves again to new_leaves
            if len(graph[neighbor]) == 1:
                new_leaves.append(neighbor)
        # store both prev_leaves(from last iteration), leaves(curr iteration)
        # and move on to a new leaves cutting process
        prev_leaves, leaves = leaves, new_leaves   
        
    # prev_leaves <- leaves (latest)
    return prev_leaves
    

```

