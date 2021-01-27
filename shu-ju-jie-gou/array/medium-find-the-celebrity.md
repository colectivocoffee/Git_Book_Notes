# \[Medium\] Find the Celebrity

\*\*\*\*[**\[Medium\] Find the Celebrity**](https://leetcode.com/problems/find-the-celebrity/)     \(1362/153\)  
Suppose you are at a party with `n` people \(labeled from `0` to `n - 1`\), and among them, there may exist one celebrity. The definition of a celebrity is that all the other `n - 1` people know him/her, but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information about whether A knows B. You need to find out the celebrity \(or verify there is not one\) by asking as few questions as possible \(in the asymptotic sense\).

You are given a helper function `bool knows(a, b)` which tells you whether A knows B. Implement a function `int findCelebrity(n)`. There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return `-1`.

### 1. Draw Graph, Brute Force: O\(N\) / O\(N\)

Time Complexity: O\(N\)    
Space Complexity: O\(N\)  storing the results of the n−1 calls to the `know(...)` API to find the celebrity.

![](../../.gitbook/assets/find_celebrity.jpg)

![&#x6EFF;&#x8DB3;&#x689D;&#x4EF6;2    /   &#x6EFF;&#x8DB3;&#x689D;&#x4EF6;3](../../.gitbook/assets/image%20%287%29.png)

滿足條件2： 只要有任何一條發散紅線成立，則4不是celebrity。  
滿足條件3： 只要缺少任何一條聚斂綠線，則4不是celebrity。

```python
def findCelebrity(self, n: int) -> int:
    
    himself = 0
    # 最重要的部分
    # 如果 himself -knows-> other，則代表himself不是celebrity，所以往other看。    
    for other in range(n):        
        if knows(himself, other):            
            himself = other  
            
    # 滿足條件2: 如果此人(himself)認識任何其他人，則他不是celebrity。
    # (從himself往所有其他地方發散出去, for other in range(himself))     
    if any(knows(himself, other) for other in range(himself)): 
        return -1
        
    # 滿足條件3: 如果此人並沒有被所有人認識，則他不是celebrity。                     
    # (從所有人聚斂到himself， for other in range(n))
    if any(not knows(other, himself) for other in range(n)):
        return -1
        
    return himself

```

