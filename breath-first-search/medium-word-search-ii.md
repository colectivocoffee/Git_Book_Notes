# \[Medium\] Word Search II

## Thought Process

### DFS Recursion + Trie

> 思路：这一题的基本思路与Word Search是相似的，都是需要寻找在一个alphabet soup中是否存在word，可以用DFS recursion的办法来寻找。不过对于Word Search II，因为事先存在一个words dictionary，所以可以利用Trie的数据结构来存储，提高查询效率。

參考答案Java

```java
class TrieNode {
    char c;
    HashMap<Character, TrieNode> children;
    boolean isLeaf;
    String s;

    public TrieNode() {
        this.children = new HashMap<Character, TrieNode>();
        this.s = "";
    }

    public TrieNode(char c) {
        this.c = c;
        this.s = "";
        this.children = new HashMap<Character, TrieNode>();
        this.isLeaf = false;
    }
}

class Trie {
    public TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word) {
        HashMap<Character, TrieNode> children = root.children;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);

            TrieNode t;
            if (children.containsKey(c)) {
                t = children.get(c);
            } else {
                t = new TrieNode(c);
                children.put(c, t);
            }

            children = t.children;

            if (i == word.length() - 1) {
                t.isLeaf = true;
                t.s = word;
            }
        }
    }

    public boolean search(String word) {
        TrieNode t = searchTrieNode(word);
        if (t != null && t.isLeaf) {
            return true;
        } else {
            return false;
        }
    }

    public TrieNode searchTrieNode(String str) {
        HashMap<Character, TrieNode> children = root.children;
        TrieNode t = null;

        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            if (children.containsKey(c)) {
                t = children.get(c);
                children = t.children;
            } else {
                return null;
            }
        }

        return t;
    }
}

public class Solution {
    int[] dirX = {0, 0, 1, -1};
    int[] dirY = {-1, 1, 0, 0};

    public void searchWord(char[][] board, int i, int j, TrieNode root, ArrayList<String> ans) {
        if (root.isLeaf == true) {
            if (!ans.contains(root.s)) {
                ans.add(root.s);
            }
        }
        if (i >= 0 && i < board.length &&
            j >= 0 && j < board[0].length &&
            board[i][j] != '#' && root != null) {

            if (root.children.containsKey(board[i][j])) {
                for (int k = 0; k < 4; k++) {
                    int x = i + dirX[k];
                    int y = j + dirY[k];
                    char temp = board[i][j];
                    board[i][j] = '#';
                    searchWord(board, x, y, root.children.get(temp), ans);
                    board[i][j] = temp;
                }
            }
        }
    }
    /**
     * @param board: A list of lists of character
     * @param words: A list of string
     * @return: A list of string
     */
    public ArrayList<String> wordSearchII(char[][] board, ArrayList<String> words) {
        ArrayList<String> ans = new ArrayList<String>();

        Trie trie = new Trie();
        for (String word : words) {
            trie.insert(word);
        }

        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                searchWord(board, i, j, trie.root, ans);
            }
        }

        return ans;
    }
}
```

