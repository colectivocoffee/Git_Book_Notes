# \[Hard\] Trapping Rain Water

## \[Hard\] Trapping Rain Water    \(9945/151\)



### 1. Expand from Center Brute Force: O\(N^2\) / O\(N\)

> 思路：  
> 1\) Trap Rain Water 需要有左右壁\(`max_left`&`max_right`\)來決定接住多少`index i`這一格的水。  
> 2\) 接水的水量侷限於 ****$$壁高(min(max\_left, max\_right)) - 底座高(height[i]) $$ ****。  
> 流程：  
> step1 利用從`index i`往左右分別展開的方式，找到左右邊最大\(`max_left & max_right`\)，  
> step2 左右邊看哪個相對較小，減去原本的height\[i\]高度，即可得到此格的水量`potential_fill`。  
> step3 把此格水量依次加到答案上。

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

