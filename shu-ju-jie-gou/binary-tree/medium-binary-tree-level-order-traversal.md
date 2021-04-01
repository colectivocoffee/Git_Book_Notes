# \[Medium\] Level Order/N-ary Tree Level Order/Zigzag Level Order/Right Side View/Vertical Order

## [\[Medium\] Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)    \(4343 / 108\)

Given a binary tree, return the level order traversal of its nodes' values. \(ie, from left to right, level by level\).

```text
Example
    3             [ 
   / \              [3],
  9  20     ==>    [9,20],
    /  \           [15,7]
   15   7         ]
```

### 1. BFS Iterative + Level Order\(curr\_level\): O\(logn\) / \(Recommend\)

> 思路：Top-Down Approach  
> 用Queue來記錄這一層除了curr之外，即將要掃的所有left&right nodes都放到裡面。當掃到curr node時，把curr添加到curr\_level中。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def levelOrder(self, root: TreeNode) -> List[List[int]]:
    
    # init result list 
    result = []
    
    if not root:
        return result
            
    queue = collections.deque()
    # start from root
    queue.append(root)
    
    # BFS + level order
    while len(queue) != 0:
        # since it looks like 
        # [[2,3],
        #  [4],
        #  [5,6]]
        # we need curr_level to store current level as list
        curr_level = []
        
        for _ in range(len(queue)):
            curr = queue.popleft()
            curr_level.append(curr.val) #易錯點 curr.val
            if curr.left != None:
                queue.append(curr.left)
            if curr.right != None:
                queue.append(curr.right)
        # curr_level finished, then add it to final result
        result.append(curr_level)
    return result
```

### 2. DFS Recursive + PreOrder Traversal:  O\(N\) / O\(N\)

> 思路：Top-Down Approach，一層一層往下掃。  
> 由於是Binary Tree，  
> dfs\(level+1, curr.left\)  
> dfs\(level+1, curr.right\)  
> 會先把同一層的左右nodes都掃完後，才會繼續往下一層走。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def levelOrder(self, root: TreeNode) -> List[List[int]]:
    result = []
    return self.dfs(0, root, result)
    
def dfs(self, level, curr, result):
    # 遞歸出口，即後面沒有node了
    if not curr:
        return 
    # 到了下一層發現還沒有curr_level，新增一個 []
    if level == len(result):
        result.append([])
    
    #    3
    #   / \
    #  9  20
    #    /  \
    #   15   7
    #[[]]
    #[[3]] 3
    #[[3], []]
    #[[3], [9]] 9
    #[[3], [9, 20]] 20
    #[[3], [9, 20], []]
    #[[3], [9, 20], [15]] 15
    #[[3], [9, 20], [15, 7]] 7
    # Add curr to curr_level
    result[level].append(curr.val)
    self.dfs(level+1, curr.left, result)
    self.dfs(level+1, curr.right, result)    
    
```

### 3. DFS Iterative + Preorder:    O\(N\) / O\(logN\)-O\(N\)

和Binary Tree Preorder Traversal寫法相同，可以參考 [Binary Tree Template](https://app.gitbook.com/@iscolectivo/s/algonote/~/drafts/-MVYHh-nlo0xTQf7btOJ/shu-ju-jie-gou/binary-tree)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
def levelOrder(self, root: TreeNode) -> List[List[int]]:
    
    result = []
    if not root:
        return result
    
    stack = []
    stack.append((root,0))
    while len(stack) != 0:
        curr, level = stack.pop()  # pop the last item on right
        if level == len(result):
            result.append([])
        
        result[level].append(curr.val)
        # 注意，這裡是先right後left
        # 因為 stack.pop()是LIFO 
        # 這樣才能    In: right->left 
        #           Out:        left -> right 
        if curr.right != None:
            stack.append((curr.right, level+1))
        if curr.left != None:
            stack.append((curr.left, level+1))
    
    return result
```

## [\[Medium\] N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/)        \(875/61\)

Given an n-ary tree, return the level order traversal of its nodes' values.  
_Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value \(See examples\)._  


![](../../.gitbook/assets/image%20%2828%29.png)



```text
Input: root = [1,null,3,2,4,null,5,6]
Output: [[1],[3,2,4],[5,6]]
```

### 2. DFS Recursive + Preorder

```python
def levelOrder(self, root: 'Node') -> List[List[int]]:

    result = []
    return self.dfs(root, result, 0)

def dfs(self, curr, result, level):

    if not curr:
        return result

    if level == len(result):
        result.append([])

    result[level].append(curr.val)
    for child in curr.children:
        self.dfs(child, result, level+1)

    return result
```

### 3. DFS Iterative + Preorder: O\(N\) / O\(logN\)-O\(N\)

Space Complexity: O\(logn\) average case and O\(n\) worst case. The space used is on the runtime stack.

```python
def levelOrder(self, root: 'Node') -> List[List[int]]:

    result = []
    if not root:
        return result

    stack = [(root,0)]

    while stack:

        curr, level = stack.pop()

        if level == len(result):
            result.append([])

        result[level].append(curr.val)
        for child in reversed(curr.children):
            stack.append((child, level+1))

    return result
```

## [\[Medium\] Binary Tree Zigzag Level Order Traversal ](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)          \(3218/726\)

Given a binary tree, return the zigzag level order traversal of its nodes' values. \(ie, from left to right, then right to left for the next level and alternate between\).

For example:  
Given binary tree `[3,9,20,null,null,15,7]`

```text
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:

```text
[
  [3],
  [20,9],
  [15,7]
]
```

### 1.  BFS Iterative, Level Order:    O\(N\) / O\(N\)

> 思路：看到題目要求是Level Order Traversal，我們就可以用Binary Tree BFS分層遍歷的特性來解題。  
> 唯一需要注意的是題目要求zigzag，我們需要reverse level % 2 == 0的值。

![](../../.gitbook/assets/image%20%2840%29.png)

Time Complexity: `O(N)`, where N is the number of nodes in the tree.  
In addition, the insertion operation on either end of the deque takes a constant time, rather than using the array/list data structure where the inserting at the head could take the `O(K)` time where K is the length of the list.

```python
def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
    if not root:
        return []
        
    result = []
    queue = deque()
    queue.append(root)
    
    level = 0
    while len(queue) != 0:
        curr_level = []
        level += 1
        for _ in range(len(queue)):
            curr = queue.popleft()
            curr_level.append(curr)
            # append all the childrens to the queue
            if curr.left != None:
                queue.append(curr.left)
            if curr.right != None:
                queue.append(curr.right)
        # 需要zigzag，因此用 % 來取remainder看level
        if level % 2 == 0:
            result.append(curr_level[::-1])
        else:
            result.append(curr_level)
    return result        
```

### 2. BFS Iterative, Level Order:   O\(N\)  / O\(N\)

由於法ㄧreverse整個list需要O\(n\)的時間，我們可以利用deque\(\)特性，直接依照正序or反序的方式以O\(1\)時間插入。這樣可以減少一些時間。

![](../../.gitbook/assets/image%20%2837%29.png)

```python
def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:

    result = []
    if not root:
        return result

    stack = [(root,0)]

    while stack:
        curr, level = stack.pop()
        if level == len(result):
            new_level = collections.deque()
            result.append(new_level)

        if level % 2 == 0:
            result[level].append(curr.val)
        elif level % 2 != 0:
            result[level].appendleft(curr.val)

        if curr.right:
            stack.append((curr.right, level + 1))
        if curr.left:
            stack.append((curr.left, level + 1))

    return result
```

### 3. DFS Recursive, Preorder:    O\(N\) / O\(H\)

這種方法取決於Binary Tree的高度，如果是Balanced Binary Tree的話Space Complexity可以到O\(H\)。

Space Complexity: O\(H\) H is the height of the tree. \(the number of levels in the tree, which would be roughly $$\log_2{N}$$ ​\)

```python
def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:

    result = []
    if not root:
        return result

    def dfs(curr, level):
        if not curr:
            return

        if level == len(result):
            curr_level = collections.deque()
            result.append(curr_level)
        # 根
        if level % 2 == 0:
            result[level].append(curr.val)
        elif level % 2 != 0:
            result[level].appendleft(curr.val)
        
        # 左 -> 右
        dfs(curr.left, level + 1)    
        dfs(curr.right, level + 1)    

    dfs(root, 0)
    return result
```

## [\[Medium\] Binary Tree Right Side View ](https://leetcode.com/problems/binary-tree-right-side-view/)     \(3631/197\)

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.

![](../../.gitbook/assets/image%20%2839%29.png)

#### Which one to choose, BFS or DFS?

* The problem is to **return a list of the last elements from all levels**, so it's way more natural to implement BFS here.
* Time complexity is the same `O(N)` both for DFS and BFS since one has to visit all nodes.
* Space complexity is `O(H)` for DFS and `O(D)` for BFS, where HH is a tree height, and D is a tree diameter. They both result in `O(N)` space in the worst-case scenarios: skewed tree for DFS and complete tree for BFS.

BFS wins for this problem, so let's use the opportunity to check out three different BFS implementations with the queue.

> 思路：Right Side View其實就是**紀錄Binary Tree每層的最後一個node。**  
> \(1\)在`level == len(right_side_result)`時就可以紀錄答案，等同於Level Order Traversal。  
> \(2\)stack需要紀錄`(node, level)`，因此初始是`stack = [(root, 0)]`

### 1. BFS Iterative, Level Order:    O\(N\) / O\(H\)

這題跟所有的Level Order相同。唯一不一樣的是，我們要right side view，因此根據stack LIFO，我們需要append先左後右。

Space Complexity: `O(H)` where H is a tree height/diameter. Let's use the last level to estimate the queue size. This level could contain up to N/2 tree nodes in the case of [complete binary tree](https://leetcode.com/problems/count-complete-tree-nodes/).

```python
def rightSideView(self, root: TreeNode) -> List[int]:

    result = []
    if not root:
        return result

    stack = [(root,0)]
    while stack:
        curr, level = stack.pop()
        if curr:
            # right side view -> 
            # To return a list of the "last" elements from all levels
            if level == len(result):                
                result.append(curr.val)
            # 由於是right side view，
            # 根據stack LIFO原則，我們要pop'先右'後左 -> 因此append先左後右
            if curr.left:
                stack.append((curr.left, level + 1))
            if curr.right:
                stack.append((curr.right, level + 1))

    return result
```

### 2. BFS Recursive, Level Order: O\(N\) / O\(H\)

```python
def rightSideView(self, root: TreeNode) -> List[int]:

    result = []
    if not root:
        return result

    def bfs(curr, level):
        if not curr:
            return
        # right side view -> 
        # To return a list of the last elements from all levels
        if level == len(result):
            result.append(curr.val)
        
        # pop先右後左
        bfs(curr.right, level + 1)
        bfs(curr.left, level + 1)

    bfs(root, 0)
    return result
```

## [\[Medium\] Binary Tree Vertical Order Traversal](https://leetcode.com/problems/binary-tree-vertical-order-traversal/)    \(1421/195\) locked

Given the `root` of a binary tree, return _**the vertical order traversal** of its nodes' values_. \(i.e., from top to bottom, column by column\).

If two nodes are in the same row and column, the order should be from **left to right**.

![](../../.gitbook/assets/image%20%2863%29.png)

![](../../.gitbook/assets/image%20%2869%29.png)



```text
Ex1
Input: root = [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]

Ex2
Input: root = [3,9,8,4,0,1,7]
Output: [[4],[9],[3,0,1],[8],[7]]
```

### 1. BFS Iterative, Queue +  DefaultDict Table:   O\(N\) / O\(N\)

> 如下圖  
> STEP1. 建立DefaultDict  col-value的對應關係  
> 以root為起始點\(0,0\)，往下走並且紀錄其\(num, column\)到queue中。  
> STEP2. 做BFS，同時往左往右延展`min_col` & `max_col`。  
> STEP3. 把DefaultDict按照col，轉換成答案要求List\(List\(\)\)

![](../../.gitbook/assets/image%20%2867%29.png)

```python
def verticalOrder(self, root: TreeNode) -> List[List[int]]:
    
    if not root:
        return root
    table = defaultdict(list)
    min_col, max_col = 0, 0
    
    queue = collections.deque()
    queue.append((root,0))
    while queue:
        curr, col = queue.popleft()
        if curr:
            table[col].append(curr.val)
            min_col = min(min_col, col)
            max_col = max(max_col, col)           
            
            queue.append((curr.left, col - 1))
            queue.append((curr.right, col + 1))
    
#        print(table)
    result = [table[x] for x in range(min_col, max_col + 1)]
    return result
```

