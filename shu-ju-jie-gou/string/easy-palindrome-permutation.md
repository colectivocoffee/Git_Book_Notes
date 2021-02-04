# \[Easy\] Palindrome Permutation

## \[Easy\] Palindrome Permutation \(526/59\)

Given a string, determine if a permutation of the string could form a palindrome.

> 思路：Palindrome有個特性：所有的chars都為偶數個\(才能左右對消\)，除了最中間的char之外。  
> 因此，我們可以數chars出現個數，如果有`超過一個`的non palin count的話，就代表不是palindrome。

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
    # 只允組至多一組不符合條件的char，超過就return False
    return non_palin_count <= 1
```

### 2. Dictionary One Pass: O\(N\) / O\(1\)

```python
def canPermutePalindrome(self, s: str) -> bool:

    count = 0
    #易錯點：用collections.defaultdict(int)並且放int在裡面
    chars_count = collections.defaultdict(int)
    for char in s:
        # 不管怎樣，都先+=1
        chars_count[char] += 1
        # 算有多少組不符合條件的non palin count pairs.
        # 符合條件則減一組，反之則增一組。
        if chars_count[char] % 2 == 0:
            count -= 1
        else:
            count += 1

    return count <= 1
```

### 3. Set: O\(N\) / O\(1\)

用set\(\)來紀錄已看過的chars。如果看過，就set.remove\(char\)；反之，如果沒看過，就set.add\(char\)。這樣一來，一加一減的方式，就等同於 count %2== 0 的功能。

```python
def canPermutePalindrome(self, s: str) -> bool:

    chars_count = set()
    for char in s:
        if char in chars_count:
            chars_count.remove(char)
        else:
            chars_count.add(char)

    return len(chars_count) <= 1
```

