# \[Hard\] Find Median from Data Stream

## [\[Hard\] Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)           \(3983/74\)

The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.

* For example, for `arr = [2,3,4]`, the median is `3`.
* For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.

Implement the MedianFinder class:

* `MedianFinder()` initializes the `MedianFinder` object.
* `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
* `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.

### 1. statistics.median:   \(TLE\)

```python
class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.arr = []

    def addNum(self, num: int) -> None:
        self.arr.append(num)

    def findMedian(self) -> float:
        return statistics.median(self.arr)
```

### 2. General Sort:  O\(NlogN\) / O\(N\)

```python
class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.arr = []

    def addNum(self, num: int) -> None:
        self.arr.append(num)

    def findMedian(self) -> float:
        self.arr.sort()
        
        length = len(self.arr) - 1
        
        # odd number of items
        if length % 2 == 0:
            return self.arr[length//2]
        # even number of items
        # [1,2,3,4] 
             ^ ^    
        # (len//2) & (len//2 + 1)
        return (self.arr[length//2] + self.arr[length//2+1])/2.0
```

### 3. Heapq:  O\(logN\) / O\(N\)

Time Complexity: O\(5\*logN\) + O\(1\)$$≈$$ O\(logN\)  
there are 3 heap insertions and 2 heap deletions on top. Each of them takes O\(logN\) time.  
Space Complexity: O\(N\) 

From the general sort solution we could realize that: 

1. If we could maintain direct access to median elements at all times, then finding the median would take a constant amount of time.
2. If we could find a reasonably fast way of adding numbers to our containers, additional penalties incurred could be lessened.

But perhaps the most important insight, which is not readily observable, is the fact that we _only_ need a consistent way to access the median elements. **Keeping the** _**entire**_ **input sorted is not a requirement. We only need two numbers min/max to maintain this.** 

> 我們發現並不需要維持整個list sorted，只需要median elements的左右邊，即max & min，兩個相除，就可以得到答案。
>
> * A max-heap to store the smaller half of the input numbers  -&gt; All the numbers in the max-heap are smaller or equal to the top element of the max-heap
> * A min-heap to store the larger half of the input numbers -&gt; All the numbers in the min-heap are larger or equal to the top element of the min-heap

```python
from heapq import heappush, heappop
class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.minHeap = []   #minHeap to store larger half
        self.maxHeap = []   #maxHeap to store smaller half
        

    def addNum(self, num: int) -> None:
        
        heappush(self.maxHeap, -num)
        smallest = heappop(self.maxHeap)
        heappush(self.minHeap, -smallest)
        if len(self.minHeap) > len(self.maxHeap):
            pop_min = heappop(self.minHeap)
            heappush(self.maxHeap, -small)

    def findMedian(self) -> float:
        if len(self.maxHeap) == len(self.minHeap):
            return (self.minHeap[0] - self.maxHeap[0])/2.0
        return -self.maxHeap[0]
```

### **Follow up Qs**

* If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution? Ans: 
  * We can **bucket sort** if the range of value is predefined. So we can initiate an **array with size 100** and a **count variable**.  addNum\(\) would just increment the value in the corresponding bucket and the count variable.  findMedian\(\) you can iterate all the buckets\(fixed size\) and find the one median is in which costs O\(1\).
* If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution? Ans: 
* `[minHeap/maxHeap Follow Up]` What if the data is very large?  
  Ans:  sampling/statistical methods that not to keep all data in memory. 

  * **External merge sort** - O\(n log n\) You basically sort the numbers on the first pass, then find the median on the second.
  * **Order statistics distributed selection algorithm** - O\(n\) Simplify the problem to the original problem of finding the kth number in an unsorted array.
  * **Counting sort histogram** O\(n\) You have to assume some properties about the range of the numbers - can the range fit in the memory? Reference: [http://www.fusu.us/2013/07/median-in-large-set-across-1000-servers.html](http://www.fusu.us/2013/07/median-in-large-set-across-1000-servers.html)

**Further Thoughts**

There are so many ways around this problem, that frankly, it is scary. Here are a few more that I came across:

* **Buckets!** If the numbers in the stream are statistically distributed, then it is easier to keep track of buckets where the median would land, than the entire array. Once you know the correct bucket, simply sort it find the median. If the bucket size is significantly smaller than the size of input processed, this results in huge time saving. [@mitbbs8080](https://leetcode.com/mitbbs8080/) has an interesting implementation [here.](https://leetcode.com/problems/find-median-from-data-stream/discuss/74057/Tired-of-TWO-HEAPSET-solutions-See-this-segment-dividing-solution-%28c%2B%2B%29)
* **Reservoir Sampling.** Following along the lines of using buckets: if the stream is statistically distributed, you can rely on Reservoir Sampling. Basically, if you could maintain just one good bucket \(or _reservoir_\) which could hold a representative sample of the entire stream, you could estimate the median of the entire stream from just this one bucket. This means good time and memory performance. Reservoir Sampling lets you do just that. Determining a **"good"** size for your reservoir? _Now, that's a whole other challenge._ A good explanation for this can be found in [this StackOverflow answer.](http://stackoverflow.com/a/10693752/2844164)
* **Segment Trees** are a great data structure if you need to do a lot of insertions or a lot of read queries over a limited range of input values. They allow us to do all such operations _fast_ and in roughly the _same amount of time_, **always.** The only problem is that they are far from trivial to implement. Take a look at my [introductory article on Segment Trees](https://leetcode.com/articles/a-recursive-approach-to-segment-trees-range-sum-queries-lazy-propagation/) if you are interested.
* **Order Statistic Trees** are data structures that seem to be tailor-made for this problem. They have all the nice features of a BST, but also let you find the k^{th}kth order element stored in the tree. They are a pain to implement and no standard interview would require you to code these up. But they are fun to use if they are already implemented in the language of your choice.

1. [Hinting](http://en.cppreference.com/w/cpp/container/multiset/insert) can reduce that to amortized constant O\(1\)O\(1\) time. [↩︎](https://leetcode.com/problems/find-median-from-data-stream/solution/#fnref4)
2. [**GNU** `libstdc++`](https://gcc.gnu.org/onlinedocs/libstdc++/manual/policy_based_data_structures_test.html) users are in luck! Take a look at [this StackOverflow answer.](http://stackoverflow.com/a/11228573/2844164) [↩︎](https://leetcode.com/problems/find-median-from-data-stream/solution/#fnref5)

\*\*\*\*

