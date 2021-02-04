# \[Easy\] Palindrome Permutation

## \[Easy\] Palindrome Permutation \(526/59\)



Palindrome有個特性：所有的chars都為偶數個\(才能左右對消\)，除了最中間的char之外。  
因此，我們可以數chars出現個數，如果有`超過一個`的non palin count的話，就代表不是palindrome。

### 1. Dictionary: O\(N\) / O\(1\)

用Dictionary方法的話，需要過兩遍，一遍是放到dictionary裡，第二遍是看有多少個non palin count，超過一個就return False。

Space Complexity: O\(1\) distinct chars are bounded. Any methods that use hashmap/dictionary or set should have space complexity O\(1\) as the char number should be less than 256 as the assumption in the O\(128\) or O\(256\)

```python
def canPermutePalindrome(self, s: str) -> bool:

    counts = {}
    for char in s:
        counts[char] = counts.get(char, 0) + 1

    non_palin_count = 0
    for count in counts.values():
        if count % 2 != 0:
            non_palin_count += 1

    return non_palin_count <= 1
```

