# \[Medium\] Container With Most Water

[Container With Most Water](https://leetcode.com/problems/container-with-most-water/)  
Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate \(i, ai\). n vertical lines are drawn such that the two endpoints of line i is at \(i, ai\) and \(i, 0\). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: ****You may not slant the container and n is at least 2.

#### Example

```text
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

## Thought Process

max\_area = max\(height \* length\) 

### 1. Brute Force: O\(n^2\)/O\(1\)

Enumerate all possible combination of`area = height * length(j-i)`using two for loop i, j. Where`length = j - i`and `curr_height = min(height[i], height[j])`。  
因此，curr\_area = length \* height 。

### 2. Two Pointers: O\(n\)

L = 左擋板index位置, R =右擋板index位置   
  
**要如何決定移動L還是R呢？**  
我們要移動兩個擋板中，比較短的那個。因為假設移了長的，整個curr\_area還是決定於原來的短擋板，並沒有任何好處。因此，我們只有**移動較短**的那個，才有讓矩形面積變大的可能。  
  
決定 `L+= 1, R -= 1`擋板的寫法如下

```python
if height[L] < height[R]:
    L += 1
else:
    R -= 1
```

{% hint style="info" %}
易錯點：1. 移動**較短**的那個擋版，即 height\[L\] &lt; height\[R\] ---&gt; L += 1，  
反之height\[L\] &gt; height\[R\]  ---&gt; R -= 1
{% endhint %}

## Code

#### 1. Brute Force: O\(n^2\)/O\(1\)

{% tabs %}
{% tab title="Python" %}
```python
def maxArea(self, height: List[int]) -> int:
    
    max_area = 0
    
    for i in range(len(height)):
        for j in range(i+1, len(height)):
            length = j - i
            curr_area = length * min(height[i], height[j])
            # compare which one is larger? max_area or curr_area
            max_area = max(max_area, curr_area)
    
    return max_area
    
    
```
{% endtab %}
{% endtabs %}

#### 2. Two Pointers: O\(n\)/O\(1\)

{% tabs %}
{% tab title="Python" %}
```python
def maxArea(self, height: List[int]) -> int:
    
    max_area = 0
    
    L, R = 0, len(height)-1
    
    while L < R:
        length = R - L
        curr_area = length * min(height[L], height[R])
        max_area = max(max_area, curr_area)
        
        if height[L] < height[R]:
            L += 1
        else:
            R -= 1
    
    return max_area 
    
```
{% endtab %}
{% endtabs %}

