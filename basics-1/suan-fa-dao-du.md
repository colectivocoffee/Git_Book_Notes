# 算法導讀

## Python常用用法

* **Last element in a sequence:** `arr[-1]`
* **Reversing from end to start:**  iterate thru index \(1\) `for i in range(len(arr)-1, -1, -1)` start, stop, step                                 \(2\) `for i in reversed(range(len(arr)))`  iterate thru element \(3\) `for element in arr[::-1]`
* **建一個新的list並且保存其sort之前的index:**  1. `arr = enumerate(arr)`  --&gt; 先化成\(index, item\)的表示法 2. `new_arr = sorted(arr, key=lambda x: x[1])`  --&gt; sort array放到new\_arr裡, 並且保存舊有的index，意即按照arr tuple裡的第二個item排序。

> Note: `key=lambda x: x[1]` 在這表示按照x \(tuple裡第二個item x\[1\]做事\)，即  
> def anonymous function \(x\):  
>       return x\[1\]

* **Remove duplicates from a list of lists:** 有三種方法 \(1\) set \(2\) itertools.groupby \(3\) if element not in new\_list

```python
(1) set
nums = [tuple(i) for i in nums] #convert to tuple
nums = set(nums) # remove duplicates
nums = [list(i) for i in nums] #convert back to list

(2)itertools.groupby(nums)
nums = sorted(nums)
nums = list(nums for nums,_ in itertools.groupby(nums)) #need nums,_ for this

(3)not_in_new_list
for item in nums:
    if item not in new_list
        new_list.append()
```

* **List -&gt; Dict 使用 ListComprehension:**

```python
List(index,value) -> Dict

# <關鍵點> key : value = i : alist[i]
# newDict={0:2, 1:3, 2:5, 3:8}  
alist = [2,3,5,8]
newDict = { i : alist[i] for i in range(len(alist)) } 
```

* **把long statement化成多行:** \(Method 1\) Use backslash  `\`，注意後面沒有空格

```text
if x == 10 or x > 0 or \
       x < 100:
```

\(Method 2\) Use parenthesis  `,`，

```text
 print ('Wow, this also works?',
               'I never knew!')
```

* **\_\_init\_\_ , \_\_lt\_\_ , \_\_gt\_\_ , \_\_eq\_\_ 等等的operators:** 

```python
# 以linked list來舉例，下面的__init__是初始化head
def __init__(self, head):
    self.head = head

# __lt__是less than，需要返回比較答案
def __lt__(self, other):
    return self.head.val < self.other.val

# __gt__是greater than
def __gt__(self, other):
    return self.head.val > self.other.val
    
```

* **String concatenation**： 是 += 還是 ''.join\(list\[\]\) 比較好呢？是`''.join(list[])`，因為他可以避免creating a new string。

```python
# BAD
#O(n+m) or O(n^2) operation
#where n & m -> size of str1 and str2
str1 += str2    is the same as  str1 = str1 + str2

# GOOD
#O(n) operation
str1 = ''.join(list[])   # here list[] is the list(str2)
```

* **Preserve All Digits :** 當我們需要traverse thru the entire string，看到兩位數以上的數字時，我們可以藉由下面這個方法，來存所有的digits。

```python
# e.g. num = 232
for char in s:
    num = 10*num + int(char) 

```

* **Add Counts into Dictionary :** count + 1，並且如果沒有key，則放一個在dictionary裡。

```python
# e.g. s = 'fantastic'
# OrderedDict([('f', 1), ('a', 2), ('n', 1), ('t', 2), ('s', 1), ('i', 1), ('c', 1)])
chars = collections.OrderedDict()
for char in s:
    chars[char] = chars.get(char, 0) + 1
```

### Replace

* Slice Assignment  `current_list[:] = new_content` 取代目前的list，並且不增加memory。

> Slicing 和 Slice Assignment 的差別：  
> \(1\) Normal Slicing  
> `new_list = current_list[:]`  
> \(2\) Slice Assignment  
> `current_list[:] = new_content`  
> 把current\_list整個換成new content。  
> 當然，我們也可以只換部分。  
> e.g. `a = [1, 2, 3, 4]  
> a[0:2] = [0, 0]  
> a = [0, 0, 3, 4]`

## Advanced Python

1. **lambda用法**  \([https://medium.com/better-programming/lambda-map-and-filter-in-python-4935f248593](https://medium.com/better-programming/lambda-map-and-filter-in-python-4935f248593)\)
2. **zip用法**
3. **Python Decorator**

**Zip用法**  
給兩個list，把它們用zip的方式黏在一起。如果遇到不匹配數量，則用None補齊。  
Zip\(\) accepts a number of iterable objects and returns a list of tuples. Each entry in tuple contains an element from each iterable object.   
We have passed two lists objects in zip\(\) , so it will return a list of tuples, where each tuple contains an entry from both the lists. Then we created a dictionary object from this list of tuples.

```python
# list of strings
aListStrings = ['best','time','of','the','year' ]
# list of numbers 
aListNums = [0,2,3,5,6]

# create a zip object, then create dict 
# dictWords = {'best':0, 'time':2, 'of':3, 'the':5, 'year':6}
zipObj = zip(aListStrings,aListNums)  # zip creates tuple
dictWords = dict(zipObj)
```

**Python Decorator**  
A decorator is a function that takes another function as an argument.

## Time Complexity

#### 各種Data Structure的Time Complexity

{% embed url="https://wiki.python.org/moin/TimeComplexity" %}

Time Complexity是跟最內層的循環次數有關，跟有幾層for loop/while loop沒有必然相關。

## Time/Space Complexity Trade-offs

Generally, to improve the speed of a program, we can either:   
\(1\) choose a more appropriate data structure/algorithm; or   
\(2\) use more memory.   
  
The latter demonstrates a classic space vs. time tradeoff, but it is not necessarily the case that you can only achieve better speed at the expense of space. Also, note that there is often a theoretical limit to how fast your program can run \(in terms of time complexity\). For instance, a question that requires you to find the smallest/largest element in an unsorted array cannot run faster than O\(N\).

## 如何選擇合適的Data Structure?

Data structures can be augmented to achieve efficient time complexities across different operations. For example, a hash map can be used together with a doubly-linked list to achieve O\(1\) time complexity for both the `get` and `put` operation in an [LRU cache](https://leetcode.com/problems/lru-cache/).

Hashmaps are probably the most commonly used data structure for algorithm questions. If you are stuck on a question, your last resort can be to enumerate through the common possible data structures \(thankfully there aren't that many of them\) and consider whether each of them can be applied to the problem. This has worked for me sometimes.

If you are cutting corners in your code, state that out loud to your interviewer and say what you would do in a non-interview setting \(no time constraints\). E.g., I would write a regex to parse this string rather than using `split()` which may not cover all cases.



## 如何確定要用哪種算法？ 

### 1. 找題目裡的Keywords

* "Searching" xxx --&gt; Binary Search/Two Pointers
* "Explore" island --&gt; BFS/DFS
* Find "Combinations" --&gt; Recursions

### 2. 看題目的Time Complexity

通常都是可以**用Time Complexity來倒推算法**。  
例如O\(logn\)的算法，就可以幾乎確定是Binary Search。

#### \(1\) Divide and Conquer -&gt; Binary Search

> 如果題目要求要用Divide and Conquer求解，可以推斷用Binary Search。Time Complexity: O\(logn\)

### 



## Notes

```text
If input array is sorted then
- Binary search
- Two pointers

If asked for all permutations/subsets then
- Backtracking

If given a tree then
- DFS
- BFS

If given a graph then
- DFS
- BFS

If given a linked list then
- Two pointers

If recursion is banned then
If we don't know how many loops we need then (recursion->stack)
- Stack

If asked for maximum/minumum subarray/subset/options then
- Dynamic programming

If asked for top/least K items then
- Heap

If asked for common strings then
- Map
- Trie

Else
- Map/Set for O(1) time & O(n) space
- Sort input for O(nlogn) time and O(1) space
```



