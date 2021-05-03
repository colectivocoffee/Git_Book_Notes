# 如何更好地Debug Recursion

## Debug Recursion模板

```python
# 全局變量，紀錄目前Recursion層數
count = 0

# 根據總層數n，決定有多少個tabs
def printIndent(n):
   for i in range(len(n)):
      print('   ')  # a tab
```

## 用n tabs紀錄層數

```python
# 下面是需要Debug的程式
def dp(ring, i, key, j):

    if j == len(key):
        return 0
        
    result = sys.maxsize
    for k in range(len(key[j])):
        result = min(result, dp(ring, j, key, i + 1))
    
    return result
```

每進入下一層recursion深度，就會多一個tab。  
而每當碰到return時，就減少一個tab。

{% tabs %}
{% tab title="Python" %}
```python
#### 加了debug helper function

# 全局變量，紀錄目前Recursion層數
count = 0                              # +

# 根據總層數n(aka count)，決定有多少個tabs
def printIndent(n):                    # +
   for i in range(len(n)):             # +
      print('   ')  # a tab


def dp(ring, i, key, j):
    printIndent(count+=1)              # +
    print('i = ', i, 'j = ', j,'\n')   # +
    
    if j == len(key):
        printIndent(count-=1)          # +
        print('return 0\n')            # +
        return 0
        
    result = sys.maxsize
    for k in range(len(key[j])):
        result = min(result, dp(ring, j, key, i + 1))
    
    printIndent(count-=1)              # +
    print('return ', result, '\n')     # +
    return result

```
{% endtab %}

{% tab title="Java" %}
```java
int count = 0;
void printIndent(int n) {
    for (int i = 0; i < n; i++) {
        printf("   ");
    }
}

int dp(string& ring, int i, string& key, int j) {
    // printIndent(count++);
    // printf("i = %d, j = %d\n", i, j);

    if (j == key.size()) {
        // printIndent(--count);
        // printf("return 0\n");
        return 0;
    }

    int res = INT_MAX;
    for (int k : charToIndex[key[j]]) {
        res = min(res, dp(ring, j, key, i + 1));
    }

    // printIndent(--count);
    // printf("return %d\n", res);
    return res;
}
```
{% endtab %}
{% endtabs %}

![](../../.gitbook/assets/image%20%2899%29.png)

不只可以看到Recursion過程，還可以看到recursion tree。

![](../../.gitbook/assets/image%20%2896%29.png)

