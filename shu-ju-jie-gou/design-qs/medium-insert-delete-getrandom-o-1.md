# \[Medium\] Insert Delete GetRandom O\(1\)

## \[Medium\] Insert Delete GetRandom O\(1\)

### 1. Dictionary + List:   O\(1\) / O\(N\)

難點1. **如何完成O\(1\)？要選擇什麼樣的Data Structure？**  
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

難點2. **如何讓Deletey在List裡從O\(N\)降至O\(1\)?**  
To delete a value at arbitrary index takes linear time. The solution here is to **always delete the last value**:

* Swap the element to delete with the last one.
* Pop the last element out.

{% tabs %}
{% tab title="Python" %}
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
            # Get index from dict. 
            # Also preserve idx of last item from list
            id_dict, last_id_list = self.dict[val], self.list[-1]
            # Swap the element to delete with the last one
            self.list[id_dict], self.dict[last_id_list] = last_id_list, id_dict
            
            # delete/pop the last element
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
{% endtab %}

{% tab title="Java" %}
```java
class RandomizedSet {
  Map<Integer, Integer> dict;
  List<Integer> list;
  Random rand = new Random();

  /** Initialize your data structure here. */
  public RandomizedSet() {
    dict = new HashMap();
    list = new ArrayList();
  }

  /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
  public boolean insert(int val) {
    if (dict.containsKey(val)) return false;

    dict.put(val, list.size());
    list.add(list.size(), val);
    return true;
  }

  /** Removes a value from the set. Returns true if the set contained the specified element. */
  public boolean remove(int val) {
    if (! dict.containsKey(val)) return false;

    // move the last element to the place idx of the element to delete
    int lastElement = list.get(list.size() - 1);
    int idx = dict.get(val);
    list.set(idx, lastElement);
    dict.put(lastElement, idx);
    // delete the last element
    list.remove(list.size() - 1);
    dict.remove(val);
    return true;
  }

  /** Get a random element from the set. */
  public int getRandom() {
    return list.get(rand.nextInt(list.size()));
  }
}
```
{% endtab %}
{% endtabs %}

![](../../.gitbook/assets/image%20%2895%29.png)

#### `def insert(self, val):`

![](../../.gitbook/assets/image%20%2896%29.png)

#### **`def delete(self, val):`**

![](../../.gitbook/assets/image%20%2897%29.png)

#### `def getRandom(self):`

GetRandom could be implemented in \mathcal{O}\(1\)O\(1\) time with the help of standard `random.choice` in Python and `Random` object in Java.



