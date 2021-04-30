# \[Medium\] Insert Delete GetRandom O\(1\)

## \[Medium\] Insert Delete GetRandom O\(1\)

### 1. Dictionary + List:   O\(1\) / O\(N\)

題目要我們完成 Insert / Delete / GetRandom 都為O\(1\)的operation。  
能讓Insert / Delete 都為O\(1\)的數據結構，就是`dictionary/hashMap`了。  
又，GetRandom也需要O\(1\)，純dictionary需要O\(N\)，這時候我們就需要`List`的輔助，使其降至O\(1\)。

> The idea of GetRandom is to choose a random index and then to get that element.   
>   
> However, there is **no indexes in hashmap**, and hence to get true random value, one has first to convert hashmap keys in a list, that would take linear time. The solution here is to build a list of keys aside and to use this list to compute GetRandom in constant time.

| Data Structure | What do we do with it? | How to use this data structure?  |
| :--- | :--- | :--- |
| Dictionary | \(key, value\) --&gt; \(val, insert\_idx\)                                    | Used for O\(1\) Insert / Delete |
| List | index, item --&gt; \(insert\_idx, val\) | 1.Used for getting index O\(1\) at getRandom method           2.Used for insert val O\(1\) at insert method |

```python
class RandomizedSet:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.dict = dict()
        self.list = []
        
    def insert(self, val: int) -> bool:
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        """
        if val not in self.dict:
            self.dict[val] = len(self.list)    # O(1) operation for this insertion
            self.list.append(val)
            return True
        return False
        
    def remove(self, val: int) -> bool:
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        """
        if val in self.dict:
            # in order to get this operation in O(1), we need idx for both list and dict.
            last_id_dict, last_id_list = self.dict[val], self.list[-1]
            # swap
            self.list[last_id_dict], self.dict[last_id_list] = last_id_list, last_id_dict
            
            # delete the last element
            self.list.pop()
            del self.dict[val]
            return True
        return False

    def getRandom(self) -> int:
        """
        Get a random element from the set.
        """
        return choice(self.list)


# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```

