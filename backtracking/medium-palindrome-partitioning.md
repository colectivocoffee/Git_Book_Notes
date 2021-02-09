# \[Medium\] Palindrome Partitioning

## [\[Medium\] Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)  \(2972/94\)

Given a string `s`, partition `s` such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of `s`.

A palindrome string is a string that reads the same backward as forward.

```text
Example 1:
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]

Example 2:
Input: s = "a"
Output: [["a"]]
```

### 1. Backtracking: O\(N \* 2^N\) / O\(N\)

The idea is to generate all possible substrings of a given string and expand each possibility if is a potential candidate. The first thing that comes to our mind is [Depth First Search](https://en.wikipedia.org/wiki/Depth-first_search). In Depth First Search, we recursively expand potential candidate until the defined goal is achieved. After that, we backtrack to explore the next potential candidate.

The backtracking algorithms consists of the following steps:

* _**Choose**_**:** Choose the potential candidate. Here, our potential candidates are all substrings that could be generated from the given string.
* _**Constraint**_**:** Define a constraint that must be satisfied by the chosen candidate. In this case, the constraint is that the string must be a _palindrome_.
* _**Goal**_**:** We must define the goal that determines if have found the required solution and we must backtrack. Here, our goal is achieved if we have reached the end of the string.

![](../.gitbook/assets/image%20%289%29.png)

Time Complexity: **O\(N \* 2^N\)**  This is the worst-case time complexity when all the possible substrings are palindrome. e.g. `'aaaa'`  
There could be 2^N possible substrings, for each substring, it takes O\(N\) time to check palindrome.  

```python
def partition(self, s: str) -> List[List[str]]:

    result = []

    def checkPalin(s):
        return s == s[::-1]

    def dfs(curr_result, str_left):
        print(curr_result, str_left)
        # Backtrack 出口 (goal)
        # 如果str_left已掃完，就可以把目前curr_result加上去
        if not str_left:
            result.append(curr_result)
            return            
        for i in range(1, len(str_left)+1):
            sliced = str_left[:i]
            if checkPalin(sliced):
                dfs(curr_result + [sliced], str_left[i:])

    dfs([], s)
    return result
```

```python
e.g. 'aabcc'

'curr_result'        'str_left' # no more str_left, then
[]                     aabcc
['a']                   abcc
['a', 'a']               bcc
['a', 'a', 'b']           cc
['a', 'a', 'b', 'c']       c     
['a', 'a', 'b', 'c', 'c']      # append to result
['a', 'a', 'b', 'cc']          # append to result
['aa']                   bcc
['aa', 'b']               cc
['aa', 'b', 'c']           c
['aa', 'b', 'c', 'c']          # append to result
['aa', 'b', 'cc']              # append to result

result
[['a', 'a', 'b', 'c', 'c'], ['a', 'a', 'b', 'cc'], ['aa', 'b', 'c', 'c'], ['aa', 'b', 'cc']]
```

### 2. Backtracking with DP: **O\(N \* 2^N\)**  / O\(N^2\)

在Backtracking時，我們一直在重複看同樣的substring是否為palindrome。為了節省重複看的時間，我們可以利用2D的DP array來記錄是否為palindrome，`dp [i][j]`分別為`dp[start][end]`。e.g.`dp[start][end] == True`  
  
Time Complexity: O\(N \* 2^N\) 還是相同，但可以減少重複看palindrome的時間。 

![](../.gitbook/assets/image%20%2810%29.png)

