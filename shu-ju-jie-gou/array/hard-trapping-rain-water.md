# \[Hard\] Trapping Rain Water

## [\[Hard\] Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)    \(9945/151\)

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

### 1. Expand from Center Brute Force: O\(N^2\) / O\(N\)

> 思路：  
> 1\) Trap Rain Water 需要有左右壁\(`max_left`&`max_right`\)來決定接住多少`index i`這一格的水。  
> 2\) 當格i的接水水量侷限於 ****$$壁高(min(max\_left, max\_right)) - 底座高(height[i]) $$ ****。  
> 流程：  
> step1 利用從`index i`往左右分別展開的方式，找到左右邊最大\(`max_left & max_right`\)，  
> step2 左右邊看哪個相對較小，減去原本的`height[i]`高度，即可得到此格的水量`potential_fill`。    如果`potential_fill`為負值，則直接用max\(a,b\)化為0。  
> step3 把當格水量依次加到答案上，總和即為總水量。

Time Complexity: O\(N^2\)  for loop貢獻一次N，在每掃一次的時候，還需要做index slicing取`max_left & max_left`，又需要額外的N \(or more specific `height[x:y], O(y-x)`\)，總共N^2。  
Space Complexity: O\(N\)   index slicing每切一次都需要額外的N，切兩次共需要2N，簡化為N。

```python
 e.g. [ 1, 0, 3, 2, 1, 2]
  4 |         
  3 |         #
  2 |         #  #     #
  1 |___#_____#__#__#__#_
idx     0  1  2  3  4  5

  4 |max_l*  max_r
  3 |         #
  2 |     fill#  #     #
  1 |___#__w__#__#__#__#_   total=1
idx     0  1  2  3  4  5
           ^
  4 |max_l      max_r     
  3 |         #
  2 |         #  #     #
  1 |___#__w__#__#__#__#_    total=1
idx     0  1  2  3  4  5
              ^
  4 |      max_l      max_r 
  3 |         #
  2 |         #  #     #
  1 |___#__w__#__#__#__#_    total=1
idx     0  1  2  3  4  5
                 ^

  4 |      max_l      *max_r   
  3 |         #    fill
  2 |         #  #  w  #
  1 |___#__w__#__#__#__#_   total=2
idx     0  1  2  3  4  5
                    ^
  4 |      max_l      
  3 |         #
  2 |         #  #     #
  1 |___#__w__#__#__#__#_   total=2
idx     0  1  2  3  4  5
                       ^

def trap(self, height: List[int]) -> int:

    total = 0

    for i in range(1, len(height)-1):

        max_left = max(height[:i])
        max_right = max(height[i+1:])
        potential_fill = min(max_left, max_right) - height[i]
        total += max(0, potential_fill)

    return total
```

