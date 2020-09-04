# Binary Tree

Binary Tree 有分治法和遍歷法

## Binary Tree Divide&Conquer

## Binary Tree Traversal 

通常Binary Tree Traversal都會和BFS & DFS同時出現。因此建議複習Binary Tree時，搭配BFS&DFS一起練習。

### 1. Pre-order Traversal: **Root-Left-Right** \(根左右\)  

```python
Pre-order Recursive 模板

def pre_order(self, root, result):
    if not root:
        return 
    result.append(root.val)        #根
    pre_order(root.left, result)   #左
    pre_order(root.right, result)  #右
```

### 2. In-order Traversal: **Left-Root-Right** \(左根右\)

```python
In-order Recursive 模板

def in_order(self, root, result):
    if not root:
        return 
    in_order(root.left, result)     #左
    result.append(root.val)         #根
    in_order(root.right, result)    #右
```

### 3. Post-order Traversal: Left-Right-Root \(左右根\)

```python
Post-order Recursive模板

def post_order(self, root, result):
    if not root:
        return 
    post_order(root.left, result)   #左
    post_order(root.right, result)  #右
    post_order(root.val)            #根
```

