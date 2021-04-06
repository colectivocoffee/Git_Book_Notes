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
        del self.store[tail_node.key]
        self._removeFromMiddle(tail_node)
        
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

