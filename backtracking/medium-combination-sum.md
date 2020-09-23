# \[Medium\] Combination Sum

[Combination Sum](https://leetcode.com/problems/combination-sum/)  
Given a **set** of candidate numbers \(`candidates`\) **\(without duplicates\)** and a target number \(`target`\), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

Note:  
\* All numbers \(including `target`\) will be positive integers.  
\* The solution set must not contain duplicate combinations.

## Thought Process:

### 1. DFS Recursive

> 思路：由於candidates中的元素可以重複使用，因此可以用DFS Recursive來枚舉所有可能的combination sum。  
> 我們可以藉由Recursion的方式更新`remaining target = target - nums[i]`，同時更新curr\_sum把`nums[i]`加入。

```python
def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
    
    result = []
    candidates.sort() #非必要，可以把答案從小到大按序排
    self.dfs(candidates, target, result, 0, []) # 易錯點：curr_sum=[] 
    
    return result
    
def dfs(self, nums, target, result, curr_id, curr_sum):

    if target < 0:
        return 
    if target == 0:
        result.append(curr_sum)
        return 
    
    for i in range(curr_id, len(nums)):
        #在改curr_sum時，要從int->list，即[nums[i]]
        self.dfs(nums, target - nums[i], result, i, curr_sum + [nums[i]])

```

