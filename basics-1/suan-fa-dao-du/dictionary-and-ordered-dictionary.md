# \[Python\] Dict & OrderedDict \| \[Java\] HashTable & HashMap

A python dictionary is basically an implementation of a **hash table**. 

## Python Hash Table

![old version python dict using hash table](../../.gitbook/assets/image%20%2877%29.png)

![new version python dict, using doubly linked list](../../.gitbook/assets/image%20%2876%29.png)

#### Advantages of dictionary

python 3.7 version and above: When resizing dict, only the indices are updated.  
old python: When resizing, every hash/key/value entry is moved or copied during a resize.

#### Disadvantages of dictionary

\(i\) **Unordered:** Dictionaries are unordered. In cases where the order of the data is important, the Python dictionary is not appropriate.

\(ii\) **A lot more space is taken:** Python dictionaries take up a lot more space than other data structures. The amount of space occupied increases drastically when there are many Python Dictionary keys. Of course, this isn’t too much of a disadvantage because memory isn’t very expensive.  
UPDATE: 新版Python dictionary 3.7後已改進至少30-95%memory usage.   
但為什麼舊版會用很多space呢？因為原始hash table需要規劃出很多未使用空間\(24 bytes\)等待使用。新版用Doubly Linked List解決了這space consuming問題。

#### Common Functions

| **Method** | **Description** |
| :--- | :--- |
| clear\(\) | This removes all the items from the dictionary |
| copy\(\) | This method returns a copy of the Python dictionary |
| fromkeys\(\) | This returns a different directory with only the key : value pairs that have been specified |
| get\(\) | This returns the value of the key mentioned |
| items\(\) | This method returns the a thuple for every key: value pair in the dictionary |
| keys\(\) | This returns a list of all the Python dictionary keys in the dictionary |
| pop\(\) | This **removes only the key** that is mentioned |
| popitem\(\) | In the latest version, this method deletes the **most recently added** item |
| update\(\) | This method updates the dictionary with certain key-value pairs that are mentioned |
| values\(\) | This method simply returns the values of all the items in the list |

## How Python Dictionary works internally

* Python dictionaries are implemented as **hash tables**.
* Hash tables must allow for **hash collisions** i.e. even if two distinct keys have the same hash value, the table's implementation must have a strategy to insert and retrieve the key and value pairs unambiguously.
* Python `dict` uses **open addressing** to resolve hash collisions \(explained below\) \(see [dictobject.c:296-297](http://hg.python.org/cpython/file/52f68c95e025/Objects/dictobject.c#l296)\).
* Python hash table is just a contiguous block of memory \(sort of like an array, so you can do an `O(1)` lookup by index\).
* **Each slot in the table can store one and only one entry.** This is important.
* Each **entry** in the table is actually a combination of the three values: **&lt; hash, key, value &gt;**. This is implemented as a C struct \(see [dictobject.h:51-56](http://hg.python.org/cpython/file/52f68c95e025/Include/dictobject.h#l51)\).
* The figure below is a logical representation of a Python hash table. In the figure below, `0, 1, ..., i, ...` on the left are indices of the **slots** in the hash table \(they are just for illustrative purposes and are not stored along with the table obviously!\).

  ```text
    # Logical model of Python Hash table
    -+-----------------+
    0| <hash|key|value>|
    -+-----------------+
    1|      ...        |
    -+-----------------+
    .|      ...        |
    -+-----------------+
    i|      ...        |
    -+-----------------+
    .|      ...        |
    -+-----------------+
    n|      ...        |
    -+-----------------+
  ```

* When a new dict is initialized it starts with 8 _slots_. \(see [dictobject.h:49](http://hg.python.org/cpython/file/52f68c95e025/Include/dictobject.h#l49)\)
* When adding entries to the table, we start with some slot, `i`, that is based on the hash of the key. CPython initially uses `i = hash(key) & mask` \(where `mask = PyDictMINSIZE - 1`, but that's not really important\). Just note that the initial slot, `i`, that is checked depends on the _hash_ of the key.
* If that slot is empty, the entry is added to the slot \(by entry, I mean, `<hash|key|value>`\). But what if that slot is occupied!? Most likely because another entry has the same hash \(hash collision!\)
* If the slot is occupied, CPython \(and even PyPy\) compares **the hash AND the key** \(by compare I mean `==` comparison not the `is` comparison\) of the entry in the slot against the hash and key of the current entry to be inserted \([dictobject.c:337,344-345](http://hg.python.org/cpython/file/52f68c95e025/Objects/dictobject.c#l337)\) respectively. If _both_ match, then it thinks the entry already exists, gives up and moves on to the next entry to be inserted. If either hash or the key don't match, it starts **probing**.
* Probing just means it searches the slots by slot to find an empty slot. Technically we could just go one by one, `i+1, i+2, ...` and use the first available one \(that's linear probing\). But for reasons explained beautifully in the comments \(see [dictobject.c:33-126](http://hg.python.org/cpython/file/52f68c95e025/Objects/dictobject.c#l33)\), CPython uses **random probing**. In random probing, the next slot is picked in a pseudo random order. The entry is added to the first empty slot. For this discussion, the actual algorithm used to pick the next slot is not really important \(see [dictobject.c:33-126](http://hg.python.org/cpython/file/52f68c95e025/Objects/dictobject.c#l33) for the algorithm for probing\). What is important is that the slots are probed until first empty slot is found.
* The same thing happens for lookups, just starts with the initial slot i \(where i depends on the hash of the key\). If the hash and the key both don't match the entry in the slot, it starts probing, until it finds a slot with a match. If all slots are exhausted, it reports a fail.
* BTW, the `dict` will be resized if it is two-thirds full. This avoids slowing down lookups. \(see [dictobject.h:64-65](http://hg.python.org/cpython/file/52f68c95e025/Include/dictobject.h#l64)\)

#### Reference

1. [How is Python Dictionary Implemented?](https://stackoverflow.com/questions/327311/how-are-pythons-built-in-dictionaries-implemented)

## Java HashTable & HashMap

