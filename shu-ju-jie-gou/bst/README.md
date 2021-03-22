# Binary Search Tree\(BST\)

## BST 特性

![](../../.gitbook/assets/image%20%2845%29.png)

```python
    # BST特性：                                  中(root)
    # 左子樹的所有值一定都小於root.val，          /           \
    # 右子樹的所有值一定都大於root.val           小           大 
    #                                      /   \        /   \
    #                                    最小  小中    中大   最大
```

## BST Time Complexity

先講結論：Balanced BST `O(logN),` skewed BST`O(N)`，因此`O(logN)-O(N)`

For the Binary Search tree, it has **two Worst case time complexity** i.e _**minimum and maximum**_.

* When the BST is balanced then the minimum worst-case time complexity is considered and that is O\(LogN\)
* When BST is skewed then maximum worst-case time complexity is considered O\(N\).
* Where N is the height of the tree.

