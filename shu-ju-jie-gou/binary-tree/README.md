# Binary Tree

## Binary Tree Traversal 

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

