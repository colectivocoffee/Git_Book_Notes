# \[Medium\] BST Iterator

## [\[Medium\] BST Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)       \(3536/313\)

Implement the `BSTIterator` class that represents an iterator over the [**in-order traversal**](https://en.wikipedia.org/wiki/Tree_traversal#In-order_%28LNR%29) of a binary search tree \(BST\):

* `BSTIterator(TreeNode root)` Initializes an object of the `BSTIterator` class. The `root` of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
* `boolean hasNext()` Returns `true` if there exists a number in the traversal to the right of the pointer, otherwise returns `false`.
* `int next()` Moves the pointer to the right, then returns the number at the pointer.

Notice that by initializing the pointer to a non-existent smallest number, the first call to `next()` will return the smallest element in the BST.

You may assume that `next()` calls will always be valid. That is, there will be at least a next number in the in-order traversal when `next()` is called.

### 1. DFS Recursive, Inorder:   O\(N\) / O\(N\)

> 關鍵點：在Init的時候需要有一個`Inorder Traversal Helper Function`來幫助我們，看這個Helper Function需要什麼，我們就Init什麼。

Time Complexity: `O(N)` is the time taken by the constructor for the iterator. The problem statement only asks us to analyze the complexity of the two functions, however, when implementing a class, it's important to also note the time it takes to initialize a new object of the class and in this case, it would be linear in terms of the number of nodes in the BST. In addition to the space occupied by the new array we initialized, the recursion stack for the inorder traversal also occupies space but that is limited to O\(h\) where h is the height of the tree.

* `next()` would take O\(1\)
* `hasNext()` would take O\(1\)

Space Complexity: `O(N)` since we create a new array to contain all the nodes of the BST. This doesn't comply with the requirement specified in the problem statement that the maximum space complexity of either of the functions should be O\(h\) where h is the height of the tree and for a well-balanced BST, the height is usually logN. So, we get great time complexities but we had to compromise on the space. Note that the new array is used for both the function calls and hence the space complexity for both the calls is O\(N\).

```python
class BSTIterator:
    # init would take linear O(n) time
    # init takes O(n) space for new array sorted_nodes
    def __init__(self, root: TreeNode):
        self.sorted_nodes = []
        self.index = -1
        self._inorder(root)
    
    # Internal Method to do recursive inorder BST traversal
    def _inorder(self, curr):
        if not curr:
            return       
        self._inorder(curr.left)
        self.sorted_nodes.append(curr.val)
        self._inorder(curr.right)
    
    # next would take O(1) time
    def next(self) -> int:
        self.index += 1
        return self.sorted_nodes[self.index]
    
    # hasNext would take O(1) time
    def hasNext(self) -> bool:
        return self.index + 1 < len(self.sorted_nodes)
```

### 2. DFS Iterative, Inorder:   O\(N\) / O\(N\)

* Time complexity: The time complexity for this approach is very interesting to analyze. Let's look at the complexities for both the functions in the class:
  * `hasNext` is the easier of the lot since all we do in this is to return true if there are any elements left in the stack. Otherwise, we return false. So clearly, this is an O\(1\) operation every time. Let's look at the more complicated function now to see if we satisfy all the requirements in the problem statement
  * `next` involves two major operations. One is where we pop an element from the stack which becomes the next smallest element to return. This is a O\(1\) operation. However, we then make a call to our helper function `_inorder_left` which iterates over a bunch of nodes. This is clearly a linear time operation i.e. O\(N\) in the worst case. This is true.

    > However, the important thing to note here is that we only make such a call for nodes which have a right child. Otherwise, we simply return. Also, even if we end up calling the helper function, it won't always process N nodes. They will be much lesser. Only if we have a skewed tree would there be N nodes for the root. But that is the only node for which we would call the helper function.

    Thus, the amortized \(average\) time complexity for this function would still be O\(1\) which is what the question asks for. We don't need to have a solution that gives constant time operations for _every_ call. We need that complexity on average and that is what we get.  
  
    ---  
    For `next` function, each node gets pushed and popped exactly once in next\(\) when iterating over all N nodes. That comes out to 2N \* O\(1\) over N calls to next\(\), making it O\(1\) on average, or O\(1\) amortized.  
    ---
* Space complexity: The space complexity is `O(N)` \(N is the number of nodes in the tree\), which is occupied by our custom stack for simulating the inorder traversal. Again, we satisfy the space requirements as well as specified in the problem statement.

```python
'''
Inorder模板

def inorder(root):
    if not root:
        return 
    result = []
    stack = []

    while root or stack:
        while root:
            stack.append(root)  # 左左左...
            root = root.left    
        root = stack.pop()      # 節點為空，就出棧
        result.append(root.val) # inorder result
        root = root.right       # 看右子樹
    return result
'''
class BSTIterator:

    def __init__(self, root: TreeNode):
        self.stack = []
        self._inorder(root)
    
    def _inorder(self, root):
        if not root:
            return
        
        while root:
            self.stack.append(root)
            root = root.left
            
    def next(self) -> int:
        # Node at the top of the stack is the next smallest element
        curr = self.stack.pop()
        
        # Need to maintain the invariant. If the node has a right child, call the
        # helper function for the right child
        if curr.right:
            self._inorder(curr.right)
        return curr.val

    def hasNext(self) -> bool:
        return len(self.stack) > 0
```

#### 有關 `def _inorder function`:

Let's also consider a helper function that we will be calling again and again in the implementation. This function, called `_inorder` will essentially add all the nodes in the leftmost branch of the tree rooted at the given node `root` to the stack and it will keep on doing so until there is no `left` child of the `root` node. Something like the following code:

```text
def inorder_left(root):
   while (root):
     S.append(root)
     root = root.left
```

For a given node `root`, the next smallest element will _always_ be the leftmost element in its tree. So, for a given root node, we keep on following the leftmost branch until we reach a node which doesn't have a left child and that will be the next smallest element. For the root of our BST, this leftmost node would be the smallest node in the tree. Rest of the nodes are added to the stack because they are pending processing. Try and relate this with a dry run of a simple recursive inorder traversal and things will make a bit more sense.

