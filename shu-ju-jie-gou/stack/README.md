# Stack & Queue

### 何時要使用Stack?

在討論之前，我們可以先看Recursion這個方法。Recursion是自己call自己，來達到當我們不知道有多少層for loop時的一個手段。然而自己call自己，會造成Recursion遞歸深度過深的情況；因此，如果不能用自己call自己的辦法時，另外一種辦法就是用While loop + Stack來實現Recursion。  
  
由此可知，當我們要“記得”之前看過的elements，又不知道還剩下多少個時，就可以用while loop + stack來解決。  
  
Stack是LIFO\(Last in First Out\)後入先出的數據結構，根據這個特點看以臨時保存一些數據，跟top比較之後，要用到時再依序彈出來，而此時top的便是我們所要的item。常用於DFS深度優先搜索。

### Stack使用小技巧

1. **存index**：在stack push/append的時候，存index而非item本身。這是為什麼呢？因為item本身有可能是重複的，而index是unique的。因此如果存index，在下一步處理時可以用index來找回是哪個item需要做處理。因此我們常常會看到以下的形式出現： `enumerate(aList) + stack.append(i)`

```python
for idx,item in enumerate(aList):
    if xxxx:
        temp = stack.pop()
        result.append(temp)
    stack.append(idx)
```

