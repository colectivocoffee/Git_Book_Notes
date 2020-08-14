# \[Hard\] Word Ladder II

[Word Ladder II](https://leetcode.com/problems/word-ladder-ii/)  
Given two words \(_beginWord_ and _endWord_\), and a dictionary's word list, find all shortest transformation sequence\(s\) from _beginWord_ to _endWord_, such that:

1. Only one letter can be changed at a time
2. Each transformed word must exist in the word list. Note that _beginWord_ is _not_ a transformed word.

Note:

* Return an empty list if there is no such transformation sequence.
* All words have the same length.
* All words contain only lowercase alphabetic characters.
* You may assume no duplicates in the word list.
* You may assume _beginWord_ and _endWord_ are non-empty and are not the same.

## Thought Process

### 1. BFS

why should we use localVisited?

It is used to guarantee words in one level can visit some common words in the next level. For example, in the 4th level, the words are \[dog, log\], the last level is \[cog\]. In the first iteration, 'dog' goes to 'cog', localvisited will go from \[\] to \[cog\], but visited remains \[\]. In the second iteration, 'log' goes to 'cog', localvisited will go from \[\] to \[cog\]. Only after finishing these two iterations, visited will be updated to \[cog\]. So in a word, with localvisited, we can visit the next level several times and get several paths, without it, we can only get one path.

### 2. BFS + DFS

### 3. Bi-directional BFS



## Code

#### 1. BFS Only + Localized Path Set: 

```python
def findLadders(self, beginWord: str, endWord: str, wordList: List[str]) -> List[List[str]]:
    # speed up the traversal process later on
    wordList = set(wordList)
    
    # begin == end
    if endWord not in wordList or beginWord == endWord:
        return []
        
    queue = deque()
    visited = set()
    ans = []
    
    # init quque and visited
    queue.append((beginWord, [beginWord]))
    visited.add(beginWord)
    
    # construct all neighbor nodes
    possible_words = collections.defaultlist(list)
    self.get_possible_words(wordList, possible_words)
    
    # len(ans) == 0 同時要存在
    while len(queue) != 0 and len(ans) == 0:
        # to keep track of each path
        localVisited = set()
        
        for _ in range(len(queue)):
            word, path = queue.popleft()
            for i in range(len(word)):
                # 易錯點
                # intermediate word = word[:i] + '*' + word[i+1:]
                # possible_words[intermediate_word]
                for neighbor in possible_words[word[:i] + '*' + word[i+1:]]:
                    # if the full path has been found, then exit 
                    if neighbor == endWord:
                        ans.append(path + [endWord])
                    # BFS traversal
                    if neighbor not in visited:
                        queue.append(neighbor, path + [neighbor])
                        localVisited.add(neighbor) # 易錯點 localVisited
        # Update visited from localVisited
        visited = visited.union(localVisited))
    
    return ans
    
    
# utility method to construct intermediate words like below
# w*rd, wo*rd, ....    
def get_possible_words(self, wordList, possible_words):
    
    for word in wordList:
        for i in range(len(word)):
            possible_words[word[:i] + '*' + word[i+1:]].append(word)
    
```

#### 2. BFS + DFS

```python
def findLadders(self, beginWord: str, endWord: str, wordList: List[str]) -> List[List[str]]:

        
```

