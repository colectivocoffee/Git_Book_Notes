# 算法導讀

## Python常用用法

* Last element in a sequence: `arr[-1]`
* Reversing from end to start: `arr[::-1]`
* s

## Time Complexity

[https://wiki.python.org/moin/TimeComplexity](https://wiki.python.org/moin/TimeComplexity)

## Time/Space Complexity Trade-offs

Generally, to improve the speed of a program, we can either:   
\(1\) choose a more appropriate data structure/algorithm; or   
\(2\) use more memory.   
  
The latter demonstrates a classic space vs. time tradeoff, but it is not necessarily the case that you can only achieve better speed at the expense of space. Also, note that there is often a theoretical limit to how fast your program can run \(in terms of time complexity\). For instance, a question that requires you to find the smallest/largest element in an unsorted array cannot run faster than O\(N\).

## 如何選擇合適的Data Structure?

Data structures can be augmented to achieve efficient time complexities across different operations. For example, a hash map can be used together with a doubly-linked list to achieve O\(1\) time complexity for both the `get` and `put` operation in an [LRU cache](https://leetcode.com/problems/lru-cache/).

Hashmaps are probably the most commonly used data structure for algorithm questions. If you are stuck on a question, your last resort can be to enumerate through the common possible data structures \(thankfully there aren't that many of them\) and consider whether each of them can be applied to the problem. This has worked for me sometimes.

If you are cutting corners in your code, state that out loud to your interviewer and say what you would do in a non-interview setting \(no time constraints\). E.g., I would write a regex to parse this string rather than using `split()` which may not cover all cases.

