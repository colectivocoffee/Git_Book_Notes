# \[Easy\] Contains Duplicate

## \[Easy\] Contains Duplicate   \(1433/825\)



### 1. Dictionary/HashMap: O\(N\) / O\(N\)

```python
def containsDuplicate(self, nums: List[int]) -> bool:

    duplicates = {}
    for num in nums:
        duplicates[num] = duplicates.get(num, 0) + 1

    for num, count in duplicates.items():
        if count > 1:
            return True
    return False
```

### 2. Set: O\(N\) / O\(N\)

```python
def containsDuplicate(self, nums: List[int]) -> bool:

    return len(nums) != len(set(nums))
```

