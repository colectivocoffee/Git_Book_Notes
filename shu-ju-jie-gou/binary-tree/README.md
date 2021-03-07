# Binary Tree

Binary Tree 的解法有以下五種: Pre-order, In-order, Post-order Traversal, Breath First Search, Depth First Search。

通常Binary Tree Traversal都會和BFS & DFS同時出現，例如要求要level order traversal \(same as BFS\)。因此建議複習Binary Tree時，搭配BFS&DFS一起練習。

![](../../.gitbook/assets/tree-traversals-clean.png)

|  | 順序 |
| :--- | :--- |
| Pre-Order Traversal \(**根**左右\) | 1 2 4 5 3 6 7                                                          |
| In-Order Traversal \(左**根**右\) | 4 2 5 1 6 3 7 |
| Post-Order Traversal \(左右**根**\) | 4 5 2 6 7 3 1 |
| Breath-First Search / Level Order Traversal | 1 2 3 4 5 6 7 |
| Depth-First Search | 1 2 4 5 3 6 7 |

  


## Binary Tree Divide&Conquer

先讓Left Subtree和Right Subtree去解決同樣的問題，得到結果之後，再整合為整顆Binary Tree的結果。

#### 為什麼Binary Tree適合使用Divide&Conquer呢？

這樣想，把整個Binary Tree看成一個大問題，而Left Subtree 就是一個subProblem，Right Subtree 是另外一個subProblem，以此類推。而這Binary Tree就直接幫我們Divide好整個問題了。 

## Binary Tree Traversal 

透過Pre-order / In-order / Post-order / Level-order\(BFS\) / DFS Traversal 的方式，遊走於整顆Binary Tree。在遍歷的時候，加上一個variable來記錄過程中需要的curr node和計算的結果result。

### 1. Pre-order Traversal: **Root-Left-Right** \(根左右\)  

Preorder特性：第一個值一定是root根節點。

```python
Pre-order Recursive 模板
根->左->右

def pre_order(self, root, result):
    if not root:
        return 
    result.append(root.val)        #根
    pre_order(root.left, result)   #左
    pre_order(root.right, result)  #右
    return result
```

```python
Pre-order Iterative 模板
使用stack，同樣順序 根->左左左...->右

def pre_order(self, root):
    if not root:
        return 
    result = []
    stack = []
    
    while len(stack) != 0 or root != None:
        while root != None:
            result.append(root.val) # Pre-order result
            stack.append(root)
            root = root.left
        # pop from stack
        curr = stack.pop() # pop the last one
        root = curr.right
    return result
        
```

```python
Pre-order Iterative 模板二

def pre_order2(self, root):
    result = []
    stack = []
    while True:
        while root != None:
            result.append(root.val)
            stack.append(root)
            root = root.left    # root -> root.left
        # check if stack is empty
        if len(stack) == 0:
            return
        # pop the node from stack and move onto the right node
        root = stack.pop()
        root = root.right
```

### 2. In-order Traversal: **Left-Root-Right** \(左根右\)

Inorder特性：  
\(1\) **root根節點把Tree精確地分成左右子樹**，  
root.left為Left Subtree，root.right為Right Subtree。  
\(2\) **BST Inorder有序**，  
result是以**ascending order**的方式儲存，可以用result\[n-1\]的方式取第n個大的值。

```python
In-order Recursive 模板
左->根->右

def in_order(self, root, result):
    if not root:
        return 
    in_order(root.left, result)     #左
    result.append(root.val)         #根
    in_order(root.right, result)    #右
    return result
```

```python
In-order Iterative 模板
使用stack，同樣順序 左左左...->根->右

def inorder(root):
    if not root:
        return 
    result = []
    stack = []
    # 易錯點：忘了root != None
    while root != None or len(stack) != 0:
        while root != None:
            stack.append(root)
            root = root.left
        root = stack.pop()
        result.append(root.val) # inorder result
        root = root.right
    return result
```

### 3. Post-order Traversal: Right-Left-Root \(左右根\)

```python
Post-order Recursive模板
左->右->根

def post_order(self, root, result):
    if not root:
        return 
    post_order(root.left, result)   #左
    post_order(root.right, result)  #右
    result.append(root.val)            #根
    return result
```

```python
Post-order Iterative模板
左左左...->右->根
```

### 4. Binary Tree BFS / Level Order Traversal:

Binary Tree BFS和BFS的技巧一樣，使用queue來紀錄所有在同一個level上的所有nodes，直到所有nodes都遍歷完成後，才會往next level走。

如果題目要求要level-by-level order traversal，那Binary Tree BFS pattern就會是很好的解題技巧。Binary Tree BFS pushes root to the queue and then keep iterating until the queue is empty。在每一次循環的時候，我們把queue最前面的curr node popleft，並且mark as visited。移除每一個node後，我們同時把所有的children nodes都放到queue裡。

如何知道用BFS？**To traverse a tree in a level-by-level fashion \(or level order traversal\)**。  
相關的題型有：

1. Binary Tree Level Order Traversal
2. Binary Tree Zigzag Traversal

### 5. Binary Tree DFS: 



## Preorder/Inorder/Postorder 比較

```python
# Preorder
def preorder(self, root):
    result = []
    stack = []
    while root != None or len(stack) != 0:
        while root != None:
            result.append(root.val) #preorder
            stack.append(root) 
            root = root.left
        root = stack.pop()
        root = root.right
    return result

# Inorder
def inorder(self, root):
    result = []
    stack []
    while root != None or len(stack) != 0:
        while root != None:
            stack.append(root)
            root = root.left
        root = stack.pop()
        result.append(root.val) #inorder
        root = root.right
    return result
    
# Postorder
def postorder(self, root):
    
```

