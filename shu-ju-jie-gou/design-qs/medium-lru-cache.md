# \[Medium\] LRU Cache

## [\[Medium\] LRU Cache](https://leetcode.com/problems/lru-cache/)     \(8135/332\)



Design a data structure that follows the constraints of a [**Least Recently Used \(LRU\) cache**](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU).

Implement the `LRUCache` class:

* `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
* `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
* `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

#### During the interview...

The standard interview strategy for a candidate is never to rush into the implementation. If you were interviewing me, I'd follow the article and list two solutions here : 1. hashmap + linked list, O\(1\) 2. ordered dictionary = OrderedDict, O\(1\). Do you think the solutions have the best time complexity? Yes. Cool, so which one would you like me to implement?

At this moment I already gained scores for 1. data structures 2. algorithms 3. communication. And I think you'd propose me to implement the ordered dictionary one only we were running out of time at this moment. Otherwise, there is a place for your further questions like "ok, what if you'd need to implement LFU cache instead ?"

#### Why using orderDict in Python?

In a real job, using the standard library is always preferable because it is just less code to maintain.

#### Follow Up Questions:

If a candidate answers using an orderedDict, I allow them to do so. But I go in-depth on the data structure fundamentals e.g. 

1. How is the orderedDict preserving order? 
2. How is it constant time?

### 1. OrderedDict / LinkedHashMap:    O\(1\) / O\(capacity\)

* Time complexity : `O(1)` both for `put` and `get` since all operations with ordered dictionary : `get/in/set/move_to_end/popitem` \(`get/containsKey/put/remove`\) are done in a constant time.
* Space complexity : `O(capacity)` since the space is used only for an ordered dictionary with at most `capacity + 1` elements.

```python
class LRUCache:

    def __init__(self, capacity: int):
        self.store = collections.OrderedDict()
        self.capacity = capacity

    def get(self, key: int) -> int:
        if key in self.store:
            value = self.store.pop(key)   # remove old one (top one)
            self.store[key] = value       # then move it to the last
            return value
        return -1

    def put(self, key: int, value: int) -> None:
        
        # case1: key in store
        # then pop the specified one
        # later will add it to the top
        if key in self.store:
            self.store.pop(key)                
        
        # case2: key in store, but already full
        # then pop the last one
        elif len(self.store) >= self.capacity: 
            self.store.popitem(last = False)  #一般popitem會丟出最上面的
                                              #這裡popitem(last = False)會丟最下面
        # case1: in store, but popped    
        # case2: not in the store, meaning we need to create one
        # always stores at the top
        self.store[key] = value
```

### 2. Dict + DoubleLinkedList / HashMap + DoubleLinkedList:    O\(1\) / O\(capacity\)

