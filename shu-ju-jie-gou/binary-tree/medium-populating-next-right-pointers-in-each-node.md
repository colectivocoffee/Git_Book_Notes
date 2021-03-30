# \[Medium\] Populating Next Right Pointers in Each Node

## [\[Medium\] Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)    \(3211/163\)

You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```text
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

**Follow up:**

* You may only use constant extra space.
* Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

![](../../.gitbook/assets/image%20%2860%29.png)

```text
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```



### 1. BFS Level Order + Queue:     O\(N\) / O\(N\)

> 關鍵點：  
> 有別於普通的Binary Tree，這題要求我們建立起`node -> node.next`的對應關係。  
> 由Figure B可知，node -&gt; node.next就是在同一Level上，隔壁鄰居的值，因此可用Level Order。  
> 雖然在圖上容易找到.next是誰，但在真正Traversal時，由於隔壁鄰居需要倒回去才能看到。  
> 我們可以利用同時看2個Levels，Queue的倒回法，來知道到底.next的真正鄰居是誰。

![](../../.gitbook/assets/image%20%2859%29.png)

#### 如何利用BFS Queue來找curr.next?

用Queue。  
Queue裡最多存兩個Level的數字 \(一次只看兩個Level\)。第一個Level是curr；第二個Level是curr對應curr.left & curr.right的關係。當`Queue[0]`被pop時，以Queue\[0\]為基準點，我們可以知道他的`左右子樹left&right childrens`；也可以知道它的`右邊鄰居.next`s

對於Code易錯點`if i < size -1`的解釋：  
由最開始的題目圖，我們可以知道1.next是Null。因此下面是用Null來分層。  
對於Complete Balanced Binary Tree，我們知道第一層只有1個數字 \(`len(queue) = 1`\)，第二層只有兩個數字\(`len(queue) = 2`\)，第三層只有4個數字\(`len(queue) = 4`\)，以此類推。  
因此，只要我們`i < len(queue) - 1`時，代表這層還沒看完。  
一但遇到 `i == len(queue) - 1`時，代表已經遇到Null，Queue裡面已沒有東西，該往下一層走了。

![](../../.gitbook/assets/image%20%2856%29.png)

以`node 2`來看，  
當len\(queue\)=2，意即node 2所在這層Queue只有兩個items。  
第一層關係：2 & 3是隔壁鄰居, 即 `2 -右邊是-> 3`  
第二層關係：2 的左右子樹分別為 4 & 5

![](../../.gitbook/assets/image%20%2857%29.png)

![](../../.gitbook/assets/image%20%2858%29.png)

```python
# level order traversal
# this gives us who the immediate right person is

# when to call it out? not sure.
# we should return the tree node

def connect(self, root: 'Node') -> 'Node':
    
    if not root:
        return root
    
    queue = collections.deque([root])
            
    while queue:
        # 先固定queue大小
        size = len(queue)
        for i in range(size):
            curr = queue.popleft()
            
            # 易錯點
            # This check is important. We don't want to
            # establish any wrong connections. The queue will
            # contain nodes from 2 levels at most at any
            # point in time. This check ensures we only 
            # don't establish next pointers beyond the end
            # of a level
            if i < size - 1:
                curr.next = queue[0]
            
            if curr.left:
                queue.append(curr.left)
            if curr.right:
                queue.append(curr.right)
    return root
```

### 2. DFS Recursive, Level Order: 

 

