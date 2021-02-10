# \[Medium\] Longest Common Subsequence \(LCS\)

## \[Medium\] [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)   \(2539/30\)

Given two strings `text1` and `text2`, return the length of their longest common subsequence.

A _subsequence_ of a string is a new string generated from the original string with some characters\(can be none\) deleted without changing the relative order of the remaining characters. \(eg, "ace" is a subsequence of "abcde" while "aec" is not\). A _common subsequence_ of two strings is a subsequence that is common to both strings.

If there is no common subsequence, return 0.

## Thought Process

依據題意，需要找"length of their longest common..."，是求**最大/最小**的類型，因此可以用DP求解。  
至於是哪種DP呢？由於題目有`text1 & text2`，我們需要對`text2[i]`&`text2`依照順序比對每一個character，因此是Two Sequence DP。

### Two Sequence DP

**Define the State:**   
 \(1\) len\(text1\) == m，len\(text2\) == n。  
 \(2\) LCS內的每一個char對應的時候，pair連線時不能有交叉。  
 \(3\) **最後一步：**看text1&text2最後一個char是否能成為pair，可以的話就加入到LCS。  
 \(4\) **Subproblem：**  
      原始problem -- 求text1\[1,..,m\] & text2\[1,...n\] 的LCS，現在可化為subproblemb如下  
      subproblem -- 求text1前i個char\( text1\[1, .. i\] \)，text2前j個char\( text2\[1,...,j \] \)的LCS  
     即 `f[i][j] = 前i-1個, 前j-1個, （上一個state的total）f[i-1][j-1] + 1 ( 當 text1[i] == text2[j])`  
  
**Transfer Function:**  
`f[i][j] = max(f[i-1][j], f[i][j-1], f[i-1][j-1] + 1 (when text1[i] == text2[j]))  
  
condition1: i,j match  
f[i][j] = f[i-1][j-1] + 1  
  
condition2: i,j not match  
f[i][j] = max(f[i-1][j], f[i][j-1])`  
  
  
**Init/Set Boundaries:**  
邊界為text1: \(1,len\(text1\)+1\)   text2: \(1,len\(text2\)+1\)  
  
**Calculate Sequence:**  
從row0開始走，row0, row1, ...row n  
row0 :  f\[0\]\[0\], f\[0\]\[1\], ... f\[0\]\[n\]  
row1 :  f\[1\]\[0\], f\[1\]\[1\], ... f\[1\]\[n\]  
...  
row n : f\[m\]\[0\], f\[m\]\[1\], ... f\[m\]\[n\]  
  
**Answer:**  
最後LCS答案為f\[m\]\[n\]

**Time Complexity: O\(mn\)  
Space Complexity: O\(mn\)**

## Full Implementation

{% tabs %}
{% tab title="Python" %}
```python
def longestCommonSubsequence(self, text1: str, text2: str) -> int:
    
    if len(text1) == 0 or len(text2) == 0:
        return 0
        
    # [[0]*column for i in range(row)]    
    f = [[0]*(len(text2)+1) for i in range(len(text1)+1)]
    
    for i in range(1, len(text1)+1): #from 1 to len()+1
        for j in range(1, len(text2)+1):
            
            #condition1: i, j match
            if text[i-1] == text[j-1]:
                f[i][j] = f[i-1][j-1] + 1
            
            #condition2: i, j not match
            else:
                f[i][j] = max(f[i-1][j], f[i][j-1])
    
    return f[len(text1)][len(text2)]
```
{% endtab %}
{% endtabs %}

## Similar Problem

* Edit Distance

