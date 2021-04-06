# \[Python\] Dict & OrderedDict \| \[Java\] HashTable & HashMap

A python dictionary is basically an implementation of a **hash table**. 

OrderedDict is implemented with **HashMap + Doubly Linked List** internally. 

## Python Hash Table

![old version python dict using hash table](../../.gitbook/assets/image%20%2877%29.png)

![new version python dict, using doubly linked list](../../.gitbook/assets/image%20%2876%29.png)

#### Advantages of dictionary \(待修改\)

python 3.7 version and above: When resizing dict, only the indices are updated.  
old python: When resizing, every hash/key/value entry is moved or copied during a resize.

#### Disadvantages of dictionary \(待修改\)

\(i\) **Unordered:** Dictionaries are unordered. In cases where the order of the data is important, the Python dictionary is not appropriate.

\(ii\) **A lot more space is taken:** Python dictionaries take up a lot more space than other data structures. The amount of space occupied increases drastically when there are many Python Dictionary keys. Of course, this isn’t too much of a disadvantage because memory isn’t very expensive.  
UPDATE: 新版Python dictionary 3.7後已改進至少30-95%memory usage.   
但為什麼舊版會用很多space呢？因為原始hash table需要規劃出很多未使用空間\(24 bytes\)等待使用。新版用Doubly Linked List解決了這space consuming問題。

### Common Functions

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">clear()</td>
      <td style="text-align:left">This removes all the items from the dictionary</td>
    </tr>
    <tr>
      <td style="text-align:left">copy()</td>
      <td style="text-align:left">This method returns a copy of the Python dictionary</td>
    </tr>
    <tr>
      <td style="text-align:left">fromkeys()</td>
      <td style="text-align:left">This returns a different directory with only the key : value pairs that
        have been specified</td>
    </tr>
    <tr>
      <td style="text-align:left">get()</td>
      <td style="text-align:left">This returns the value of the key mentioned</td>
    </tr>
    <tr>
      <td style="text-align:left">items()</td>
      <td style="text-align:left">This method returns the a thuple for every key: value pair in the dictionary</td>
    </tr>
    <tr>
      <td style="text-align:left">keys()</td>
      <td style="text-align:left">This returns a list of all the Python dictionary keys in the dictionary</td>
    </tr>
    <tr>
      <td style="text-align:left">pop()</td>
      <td style="text-align:left">This <b>removes only the key</b> that is mentioned</td>
    </tr>
    <tr>
      <td style="text-align:left">popitem()</td>
      <td style="text-align:left">
        <p>OrderedDict.popitem()</p>
        <p>In the latest version, this method <b>deletes</b> the <b>most recently added</b> item,</p>
        <p>meaning in <b>LIFO</b> order, just like a stack.</p>
        <p>
          <br />OrderedDict.popitem(last = False)</p>
        <p>with <b>last = False</b> param, meaning in FIFO order. It deletes oldest
          added item, just like a queue.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">update()</td>
      <td style="text-align:left">This method updates the dictionary with certain key-value pairs that are
        mentioned</td>
    </tr>
    <tr>
      <td style="text-align:left">values()</td>
      <td style="text-align:left">This method simply returns the values of all the items in the list</td>
    </tr>
  </tbody>
</table>

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

| **Type** | Safety |
| :--- | :--- |
| HashTable | synchronized  \(stronger thread-safety than concurrent\)                                |
| HashMap | async |
| ConcurrentHashMap |  |

## How HashMap internally works in Java? 

* Hash Functions are used to find the object
* First Step: hashCode\(\)
* Second Step: equals\(\)

```python
1. The concept of hashing
2. Collision resolution in HashMap
3. Use of equals () and hashCode () and their importance in HashMap?
4. The benefit of the immutable object?
5. Race condition on HashMap  in Java
6. Resizing of Java HashMap
```



Q1: Have you used HashMap before or What is HashMap? Why do you use it?  
Ans: **HashMap stores key-value pairs**.

Q2: Do you Know how HashMap works in Java or How does get \(\) method of HashMap works in Java?  
Ans: HashMap works on the principle of **hashing**. Hash functions are used to find a bucket location. Java stores key-value pair objects as `Map.Entry` in a bucket.

Q3: What will happen if two different objects have the same hashcode?  
Ans: **Collision will happen**.   
We need collision resolution by either **\(1\)linked list,** **\(2\)linear probing**, or **\(3\)chaining.**

Q4: How will you retrieve the Value object if two Keys will have the same hashcode?  
Ans: By Open addressing or Closed addressing to resolve this.  
They will be **stored in the same bucket but no next node of the linked list**. And keys equals \(\) method will be used to identify the correct key-value pair in HashMap.  
HashMap stores key-value pairs in `Linked List` or `Map.Entry`.   
keys.hashcode\(\) first, then keys.equals\(\) second.   
\(STEP 1\) `Hashcode()` — keys.hashcode\(\) using **Linked List traversal** until we **find the bucket location of this key**.   
\(STEP 2\) `Equals()`  — After bucket location is found, use keys.equals\(\) to **check if it is a correct object/correct node**.

Q5: What kind of keys would be considered as good HashMap keys? or How to improve the overall HashMap performance?   
Ans: **Use** **Immutable type of keys**.   
**String** and some wrapper classes like **Integer** are good immutable keys.  

Q6: What happens on HashMap in Java if the size of HashMap exceeds a given threshold defined by load factor? or HashMap Resizing?   
e.g. **Exceeds the load factor** `.75` or by 75%, it will resize the HashMap.   
Ans: Java will have the following two steps:   
step1\) create a new bucket array of size 2x and then   
step2\) **Rehashing,** to put everything into the new one. 

Q7: Do you see any problem with resizing HashMap?   
Ans: **Race condition** during the resizing. Will create an infinite loop.   
If two threads are both trying to resize the same HashMap, a race condition will happen.   
During the resizing process, elements in the bucket in the linked list get **reversed order.** Normally Java appends the new element at the end/tail. But now it **appends at the head to avoid tail traversing**.   
Therefore, HashMap is **NOT** a good choice in a multi-threaded environment.  

Q8: Why String, Integer, and other wrapper are considered good keys?  
Ans: They are `immutable and final`. They override equals and hashCode\(\) method. \[Reason1\] **Immutability** is required in order to prevent changes in fields to calculate hashCode\(\).   
\[Reason2\] **Thread safety**. Immutability offers other advantages like thread-safety. This will also help on reducing collision and improve the HashMap performance. 

Q9: Can we use ConcurrentHashMap to replace HashTable?   
Ans: Not really. HashTable has stronger thread-safety than ConcurrentHashMap.

Q10. How Null key is handled in HashMap? Since equals\(\) and hashCode\(\) are used to store and retrieve values, how does it work in the case of the null key?  
Ans: There are two separate methods for this,   
1. method PUT`putForNullKey(V value)` and   
2. method GET `getForNullKey()`.   
Null keys always map to index 0.

