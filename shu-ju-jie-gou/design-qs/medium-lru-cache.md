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

### 1. OrderedDict / LinkedHashMap:    O\(1\) / O\(capacity\)

> 要按順序完成以下三點：
>
> 1. Get the key / Check if the key exists
> 2. Delete the first added key
> 3. Put the key

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

![](../../.gitbook/assets/image%20%2879%29.png)

One advantage of _double_ linked list is that the node can remove itself without other reference. In addition, it takes constant time to add and remove nodes from the head or tail.

One particularity about the double linked list implemented here is that there are _pseudo head_ and _pseudo tail_ to mark the boundary, so that we don't need to check the `null` node during the update.

![](../../.gitbook/assets/image%20%2876%29.png)

