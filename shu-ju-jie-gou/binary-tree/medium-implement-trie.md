# \[Medium\] Implement Trie

[Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/)  
Implement a trie with `insert`, `search`, and `startsWith` methods.  


## Trie \(音try\) 特性：

想像一下Google Search的時候，會自動補齊所有有可能的字詞。

Trie字典樹有三點性質：  
\(1\) root不包含任何char，除了root之外每個節點只包含一個char  
\(2\) 從root到某一個個節點，如果把路徑上經過的所有char連起來，為該node對應的word  
\(3\) 每個節點的所有children nodes，所包含的word都不相同，而且有可能大於2個。  


> 思路：\(1\)用`self.nodes = defaultdict()`來儲存每個char，用self.isWord來確認是否為word。  
> \(2\) `curr = curr.nodes[char]` 來往下走到children node

```python
class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        ## always start from the root
        
        """
        self.root = TrieNode()
        

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        ### insert all chars to the trie, also make the isword as True.
    
        """
        curr = self.root
        for char in word:
            curr = curr.nodes[char]  # move down to children node
        curr.isWord = True
        

    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        ### Should return True/False
        
        """
        curr = self.root
        for char in word:
            if char not in curr.nodes:
                return False
            curr = curr.nodes[char]
        return curr.isWord           # 易錯點
        

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        ### Return True/False
        
        """
        curr = self.root
        for char in prefix:
            if char not in curr.nodes:
                return False
            curr = curr.nodes[char]
        return True        
        

class TrieNode:
    
    def __init__(self):    
        self.isWord = False
        self.nodes = defaultdict(TrieNode)
```

