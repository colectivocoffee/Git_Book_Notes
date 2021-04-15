# Linked List

## Linked List核心技巧

{% hint style="info" %}
如果需要建Linked List，就是要維持**`prevNode -> node -> nextNode`**的關係。  
因此會有以下性質：  
\(1\) node = prevNode.next  
\(2\) nextNode = node.next  
\(3\) nextNode = prevNode.next.next    \(aka skip current node\)
{% endhint %}

下面是Linked List Traversal/Insert的性質

* Linked List是一串葡萄，只要拿head就會得到從head開始，所有的elements。
* 維持 head & head.next 的關係
* **DummyNode** Linked List頭部（即`ListNode(0)`）需要有Dummy Node來當Reference。這樣在return結果時，就可以用dummy.next直接回到Linked List的最頭部。 ex: **`0 ->`** 1 -&gt; 2 -&gt; 3 -&gt; 4 -&gt; 5 -&gt; Null 寫法： `Dummy = ListNode(0) Dummy.next = Head .....`
* **Skip/Remove Duplicates** : `node.next = node.next.next`
* **i += 1** :  `node = node.next` 
* **Assign Pointers**: 由於Linked List沒有index，因此需要新建立variables，來建立它和原始head的關係 `odd = head even = odd.next ....`
* **Reverse Linked List 模板:**  `temp = curr.next curr.next = temp.next temp.next = prev.next prev.next = temp`
* ssss

## 基本概念

![Array vs Linked List](../../.gitbook/assets/array-vs-linked-list.png)

### 1. Linked List vs Array

|  | **Array** | **Linked List** |
| :--- | :---: | :---: |
| 特性 | Value & Index | Value & Next \(-&gt;\)  |
| 如何取值 | arr\[index\]  \(ex: arr\[0\]\) | head.next |
| Retrieve Specific Element | O\(1\) | O\(n\) |
| Insert/Delete | O\(n\)/O\(n\) | O\(1\)/O\(1\) \(only head\) |
|  |  |  |

* 使用LInked List的好處在於，**Insert/Delete Operation只需要O\(1\)的時間複雜度**。
* LinkedList should be used where modifications to a collection are frequent like addition/deletion operations O\(1\).  \(1\) when you re-use existing iterators to insert and remove elements \(2\) when you add or remove from the head of the list Else ArrayList is good when Read Only or Data Rarely modified. 

### 2. Linked List需要注意的地方

由於Linked List取值只能用`node.next`來取。在解題時，需要實時地maintain \(1\)`head` \(2\) `head.next` 這兩者的關係。

### 3. 為什麼需要DummyNode

 `Dummy  
  [X] -> Head -> node -> node -> ....`

DummyNode 一般不存儲有意義的值，而是用於`佔位`。這樣的好處是，我們會比較方便對Head進行Insert/Delete等操作而又可以保持原始的Linked List狀態。

### 4. Linked List Basic Operations

\(1\) 往後移一格

```text
Head->Head
(1) -> 2 -> 3 -> 4 -> 5 -> None  head = head.next
       
      (2)-> 3 -> 4 -> 5 -> None
```

\(2\) Skip/Remove Duplicates 跳過，直接往下下一格走

```text
Head     ->Head
(1) -> 2 -> 3 -> 4 -> 5 -> None  head.next = head.next.next
       
           (3)-> 4 -> 5 -> None
```

\(3\) 把B LinkedList 黏到 A 上

```text
node A: (1) -> 3 -> 5 -> None
node B: (2) -> 4 -> None           A.next = B

(1) -> 3 -> 5 -> 2 -> 4 -> None
```

\(4\) 在head最前面加上一個dummy node  
      dummyNode的目的在於，讓我們方便回到原來head。

```text
     Head 
      (1) -> 2 -> 3 -> 4 -> 5 -> None   dummy = ListNode(0)
dummy                                   dummy.next = head
(0) -> 1 -> 2 -> 3 -> 4 -> 5 -> None
```

\(5\) 在原來的LinkedList上，建立一個新的pointer  
      注意：原始的Head並沒有變，Head只有在odd.next = odd.next.next這種skip方式時才會跟著變化。

```text
       Head 
       (1) -> 2 -> 3 -> 4 -> 5 -> None    odd = head
  odd: (1) -> 2 -> 3 -> 4 -> 5 -> None    
       Head                               odd = odd.next
       (1) -> 2 -> 3 -> 4 -> 5 -> None
  odd:       (2)-> 3 -> 4 -> 5 -> None 
```

```text
       Head 
       (1) -> 2 -> 3 -> 4 -> 5 -> None    odd = head
  odd: (1) -> 2 -> 3 -> 4 -> 5 -> None    
            Head                          odd.next = odd.next.next
            (1) -> 3 -> 4 -> 5 -> None
  odd:      (1) -> 3 -> 4 -> 5 -> None 
```

## Reverse Linked List \(In-Place\)

In a lot of problems, you may be asked to reverse the links between a set of nodes of a linked list. Often, the constraint is that you need to do this in-place, i.e., using the existing node objects and without using extra memory. This is where the above-mentioned pattern is useful. 

This pattern reverses one node at a time starting with one variable \(current\) pointing to the head of the linked list, and one variable \(previous\) will point to the previous node that you have processed. In a lock-step manner, you will reverse the current node by pointing it to the previous before moving on to the next node. Also, you will update the variable “previous” to always point to the previous node that you have processed.

![](../../.gitbook/assets/reversedlinkedlist.jpg)

### **Reverse Linked List的指針移動步驟**

The trick to reversing one list into another is **inserting at the head**, rather than at the back, of the target list. You need to traverse your original list in a regular way by following the `next` pointers, but **rather than adding elements to the end of the target list, create a new node, and replace the header of the target with it**.

{% hint style="info" %}
口訣：**`自己 -- 別人 -- 自己 -- 移動`**  
\(口訣上都是記等號右邊的指針情況\)  
  
temp = head.next  
\(1\) **自己** head.next  
  
head.next = new\_head  
\(2\) **別人**`new_head`  
  
new\_head = head  
\(3\) **自己** head  
  
head = temp  
\(4\) **移動** temp
{% endhint %}

STEP1. Create a Dummy Node  
STEP2. Inserting at the head of the target list  
STEP3. First node --&gt; Dummy Node  
              Second node --&gt; First node --&gt; Dummy Node  
              .... 

```python
sourceHead -> "A" -> "B" -> "C" -> NULL
your pointer   ^
targetHead -> NULL

sourceHead -> "A" -> "B" -> "C" -> NULL
your pointer          ^
targetHead -> "A" -> NULL

sourceHead -> "A" -> "B" -> "C" -> NULL
your pointer                 ^
targetHead -> "B" -> "A" -> NULL

sourceHead -> "A" -> "B" -> "C" -> NULL
your pointer                        ^
targetHead -> "C" -> "B" -> "A" -> NULL
```

```python
# 模板
def reverseList(self, head: ListNode) -> ListNode:
    # edge case
    if head == None:
        return head
    # init reverse head
    new_head = None
    
    while head != None:
        # step1. [自己] preserve head.next's value
        temp = head.next
        # step2. [別人] override head.next
        head.next = new_head
        
        # step3. [自己] move new_head
        new_head = head
        # step4. [移動] move original head
        head = temp
    
    return new_head
```

### Reverse Linked List Pattern: 

* If you’re asked to **reverse a linked list without using extra memory**

### Problems

* Reverse a Sub-list \(medium\)
* Reverse every K-element Sub-list \(medium\)
* [x] Reverse Linked List \(easy\)
* [x] Reverse Linked List II \(medium\)

