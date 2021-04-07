# \[Medium\] LRU Cache

## [\[Medium\] LRU Cache](https://leetcode.com/problems/lru-cache/)     \(8135/332\)

Design a data structure that follows the constraints of a [**Least Recently Used \(LRU\) cache**](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU).

Implement the `LRUCache` class:

* `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
* `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
* `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

#### How to handle this during the interview...

The standard interview strategy for a candidate is never to rush into the implementation. If you were interviewing me, I'd follow the article and list two solutions here :   
**1. hashmap + linked list, O\(1\)   
2. ordered dictionary = OrderedDict, O\(1\)**.   
Do you think the solutions have the best time complexity? Yes. Cool, so which one would you like me to implement?

At this moment I already gained scores for 1. data structures 2. algorithms 3. communication. And I think you'd propose me to implement the ordered dictionary one only we were running out of time at this moment. Otherwise, there is a place for your further questions like "ok, what if you'd need to implement LFU cache instead ?"

### Follow Up Questions:

If a candidate answers using an orderedDict, I allow them to do so. But I go in-depth on the data structure fundamentals e.g. 

1. How is the orderedDict preserving order?   Ans: Python uses **Doubly Linked List** implementation to keep it in order.           Starting Python 3.7, regular dict\(\) has guaranteed insertion order.           orderedDict 比 dict\(\) 慢一倍.
2. How is it constant time O\(1\)? Ans: Dict works on the principle of Hashing. It is using a **hash function** to search for the hash keys, similar to dict. 
3. Why using orderDict in Python? Ans: In a real job, using the standard library is always preferable because it is just `less code to maintain`.

## Code

![&#x7528;hashmap&#x8F14;&#x52A9;&#xFF0C;hashmap&#x7684;value&#x7D00;&#x9304;&#x6BCF;&#x500B;linked list&#x7BC0;&#x9EDE;&#x7684;&#x5F15;&#x7528;](../../.gitbook/assets/image%20%2880%29.png)

> LRU Cache is just repositioning the pointers by **adding/removing Least Recently Used** items. Change the position of the node means we need two different operations:   
> `Insert` & `Delete`.   
>   
> 要按順序完成以下三點：
>
> 1. Get the key / Check if the key exists
> 2. Delete the first added key
> 3. Put the key

**Getting**

* Whenever you are getting the value of the key, check whether the key exists in the cache.
  * If it exists, put that key-value pair at the end of the cache and return the value.
  * If it does not exist, return -1.

**Putting**

* Whenever you are putting a key-value pair in, you have to check whether the key already exists.
  * If it exists, you get that key-value pair, update the value and put that pair at the end of the cache.
  * If the key does not exist, you check whether the cache size is already at limit.
    * If the cache size is at limit, **pop** the key-value pair at the **beginning** of the cache and **push** the new key-value pair at the **end** of the cache.
    * If the cache is still under the size limit, simply push the new key-value pair at the end of the cache.

### 1. OrderedDict / LinkedHashMap:    O\(1\) / O\(capacity\)

* Time complexity : `O(1)` both for `put` and `get` since all operations with ordered dictionary : `get/in/set/move_to_end/popitem` \(`get/containsKey/put/remove`\) are done in a constant time.
* Space complexity : `O(capacity)` since the space is used only for an ordered dictionary with at most `capacity + 1` elements.

{% tabs %}
{% tab title="Python" %}
```python
class LRUCache:

    def __init__(self, capacity: int):
        self.store = collections.OrderedDict()
        self.capacity = capacity

    def get(self, key: int) -> int:
    
        # STEP1. Get the key / Check if the key exists
        if key in self.store:
            
            # STEP2. Delete the first added key
            # STEP3. Put the key
            #  
            # 第二種寫法：
            # self.store.move_to_end(key) 放到最下面,且return None
            # return self.store[key]
            value = self.store.pop(key)   # remove old one (top one)
            self.store[key] = value       # then move it to the last
            return value
        return -1

    def put(self, key: int, value: int) -> None:
        
        # STEP1. Get the key / Check if the key exists
        #
        # case1: key in store
        # then pop the specified one
        # later will add it to the top
        if key in self.store:
        
            # STEP2. Delete the first added key
            self.store.pop(key)                
        
        # case2: key in store, but already full
        # then pop the last one
        elif len(self.store) >= self.capacity: 
            self.store.popitem(last = False)  #一般popitem會丟出最上面的
                                              #這裡popitem(last = False)會丟最下面
        
        # STEP3. Put the key  
        #
        # case1: in store, but popped    
        # case2: in store, but full capacity
        # case3: not in the store, meaning we need to create one
        # this pair always stores at the top
        self.store[key] = value
```
{% endtab %}

{% tab title="Java" %}
```java
class LRUCache extends LinkedHashMap<Integer, Integer>{
    private int capacity;
    
    public LRUCache(int capacity) {
        super(capacity, 0.75F, true);
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity; 
    }
}
```
{% endtab %}
{% endtabs %}

### 2. Dict + DoublyLinkedList / HashMap + DoubleLinkedList:    O\(1\) / O\(capacity\)

The most frequent operation for this problem is to `move node position` within the data structure.

![](../../.gitbook/assets/image%20%2879%29.png)

One advantage of _double_ linked list is that the node can remove itself without other references. Just need to repoint when doing insertion/deletion. Therefore, it takes constant time to add and remove nodes from the head or tail.

One particularity about the double linked list implemented here is that there are _pseudo head_ and _pseudo tail_ to mark the boundary, so that we don't need to check the `null` node during the update.

![Doubly Linked List&#x521D;&#x59CB;&#x72C0;&#x614B;](../../.gitbook/assets/image%20%2876%29.png)

这样删除的时候，直接删除尾指针指向的结点，并删除hashmap中的pair。在挪动结点的时候是不必更新hashmap的，因为hashmap存储的是引用而非绝对位置。

```python
class ListNode:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

# LRU構造
#              _insertToHead(node)
# | head    /  
# |  | node    
# |  | node    _removeFromMiddle(node)
# |  v node    
# | tail    \  _removeFromTail()
#  -----
class LRUCache:
    
    #  head  --> tail                       
    # (-1,-1) -> (0,0)
    def __init__(self, capacity: int):
        self.store = dict()
        self.capacity = capacity
        self.head = ListNode(-1, -1)
        self.tail = ListNode(0, 0)
        # initial relationship between head & tail
        self.head.next = self.tail
        self.tail.prev = self.head

    def get(self, key: int) -> int:
        if key in self.store:
            node = self.store[key]
            self._removeFromMiddle(node)
            self._insertToHead(node)
            return node.value
        return -1
        
    def put(self, key: int, value: int) -> None:
        
        # update with the current node(key,value)
        if key in self.store:
            # pop first item on the top
            node = self.store[key]
            self._removeFromMiddle(node)
            node.value = value
    
        # remove last item, add the latest one to the top
        elif len(self.store) >= self.capacity:
            self._removeFromTail()        
        # create new node in order to insert
        node = ListNode(key, value)
        self.store[key] = node
        self._insertToHead(node)
        
        
        # ... prevNode -> node -> nextNode ....
        # ... prevNode ---------> nextNode ....
    def _removeFromMiddle(self, node):
        # get prevNode and nextNode positions first.
        prevNode = node.prev
        nextNode = node.next
        # remove node itself, and then relink both sides. 
        prevNode.next = nextNode
        nextNode.prev = prevNode

                
        # .... -> tail_node -> tail          
        # .... --------------> tail
    def _removeFromTail(self):
        if len(self.store) == 0:
            return
        tail_node = self.tail.prev
        del self.store[tail_node.key]     # 1. delete from hashMap
        self._removeFromMiddle(tail_node) # 2. delete from linked list
        
        # head -> headNext -> ....
        # head -> node -> headNext -> ....
    def _insertToHead(self, node):        
        # 從head取headNext原始位置，並且把node加到最頂端(head後面)
        headNext = self.head.next
        self.head.next = node
        # relink both sides
        # 先連 head -> node
        #          <-
        # 再連         node -> headNext
        #                  <-
        node.prev = self.head
        node.next = headNext
        headNext.prev = node
```

## [\[Hard\] LFU Cache](https://leetcode.com/problems/lfu-cache/)       \(1933/150\)

Design and implement a data structure for a [Least Frequently Used \(LFU\)](https://en.wikipedia.org/wiki/Least_frequently_used) cache.

To determine the least frequently used key, a **use counter** is maintained for each key in the cache. The key with the smallest **use counter** is the least frequently used key.

When a key is first inserted into the cache, its **use counter** is set to `1` \(due to the `put` operation\). The **use counter** for a key in the cache is incremented either a `get` or `put` operation is called on it.

## Code

和LRU Cache概念相同，就是需要一個額外的Counter來計算Frequency。

#### Here is how the algorithm works

**get\(key\)**

1. query the `node` by calling `self._node[key]`
2. find the frequency by checking `node.freq`, assigned as `f`, and query the `DLinkedList` that this node is in, through calling `self._freq[f]`
3. pop this node
4. update node's frequence, append the node to the new `DLinkedList` with frequency `f+1`
5. if the `DLinkedList` is empty and `self._minfreq == f`, update `self._minfreq` to `f+1`.
6. return `node.val`

**put\(key, value\)**

* If key is already in cache, do the same thing as `get(key)`, and update `node.val` as `value`
* Otherwise:
  1. if the cache is full, pop the least frequenly used element \(\*\)
  2. add new node to `self._node`
  3. add new node to `self._freq[1]`
  4. reset `self._minfreq` to 1

\(\*\) The least frequently used element is the **tail element in the DLinkedList with frequency self.\_minfreq**

### 1. Dict + OrderedDict + DoublyLinkedList

```python
class LRUCache(object):
    
    class ListNode(object):
        def __init__(self, k, v):
            self.k, self.v = k, v
            self.prev, self.next = None, None

    def __init__(self, capacity):
        """
        :type capacity: int
        """
        self.capacity, self.size, self.d = capacity, 0, {}
        self.head, self.tail = self.ListNode(0, 0), self.ListNode(0, 0)
        self.head.next, self.tail.prev = self.tail, self.head
    
    def __unlink_node(self, node):
        node.prev.next = node.next
        node.next.prev = node.prev
        node.prev, node.next = None, None
    
    def __insert_at_head(self, node):
        node.prev, node.next = self.head, self.head.next
        self.head.next.prev = node
        self.head.next = node

    def get(self, key):
        """
        :rtype: int
        """
        if key not in self.d:
            return -1
        node = self.d[key]
        self.__unlink_node(node)
        self.__insert_at_head(node)
        return node.v
        
    def set(self, key, value):
        """
        :type key: int
        :type value: int
        :rtype: nothing
        """
        if key in self.d:  # hit
            node = self.d[key]
            node.v = value
            self.__unlink_node(node)
            self.__insert_at_head(node)
        else:  # insert
            if self.size == self.capacity:
                least_recently_used_node = self.tail.prev
                self.__unlink_node(least_recently_used_node)
                del self.d[least_recently_used_node.k]
                self.size -= 1
            new_node = self.ListNode(key, value)
            self.__insert_at_head(new_node)
            self.d[key] = new_node
            self.size += 1
```

#### Reference

1. [LRU & LFU](https://izhen.me/2019/09/01/lru_lfu/)

