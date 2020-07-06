# \[Easy\] Maximum Subarray

[Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)  
Given an integer array `nums`, find the contiguous subarray \(containing at least one number\) which has the largest sum and return its sum.

#### Example

```text
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

#### Follow-up Question:

If you have figured out the O\(_n_\) solution, try coding another solution using the divide and conquer approach, which is more subtle.

## Thought Process

### 0. Brute Force

用for loop i & j，i代表頭, j代表尾，找`max(sum(nums[i:j]))`即可，但這Time complexity顯然不符合題目要求。Time Complexity: O\(n^2\), Space O\(n\)

### 1. DP \(Sequence DP\): O\(n\)/O\(n\)

根據題意“find the contiguous subarray which has the largest sum...”，我們可以知道是找最大/最小值的optimization problem，也就是可以用DP來解。  
由於用DP，我們就需要找subproblem。又題目說找最大值，我們可以用max\(a,b\)來比較上一個狀態，看哪一個符合。  
  
**Define the State:**  
\(1\)最後一步: `global_max = nums[i] + prev_max`  
\(2\)Subproblem: `max(nums[i] + prev_max, nums[i])` or `nums[i] + max(prev_max,0)`  
**Transfer Function:**  
 ``**`f[i] = max(nums[i] + f[i-1], nums[i])`** where i = 0, len\(nums\)-1  
**Init state and set Boundaries:**  
Init: f\[0\] = 0  
Boundaries: 0~len\(nums\)  
**Calculate Sequence and Answers:**  
f\[0\], f\[1\], ..., f\[len\(nums\)\]  
answer: `max(f)`

![last state of this problem](../.gitbook/assets/1.jpg)

### 2. DP2 \(Optimize Space\): O\(n\)/O\(1\)

### 3. Binary Search

### 4. Greedy

## Code

#### 1.DP \(Sequence DP\)

#### 2. DP\(Space Optimized\)

#### 3. Binary Search

