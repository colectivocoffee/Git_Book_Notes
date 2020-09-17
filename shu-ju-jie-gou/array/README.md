# Array

基本上是考察對於list, array這種類型的數據結構掌握度。  
  
例如 [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) \(Easy\)  
Given an array of integers, find if the array contains any duplicates.

```text
Example 1:
Input: [1,2,3,1]
Output: true

Example 2:
Input: [1,2,3,4]
Output: false
```

就可以先排序 nums = sorted\(nums\)。

## 常考類型

### 1. The array is Sorted/Partially Sorted? 

> If yes, we can use **binary search** to reach `O(logn)`

### 2. Can we sort the array?

> We can signifcantly reduce the problem complexity by `sorted(array)` first.

### 3. Summation/Multiplication of Subarray?

> For sum/product of subarray quesitons, we might need **hashing** or **prefix/suffix sum/product** to compute it first.

Maximum Subarray  
Maximum Product Subarray

### 4. Constraint on Space Complexity O\(1\)

> If we are asked to optimize space to O\(1\), probably we have to **reuse the array itself** as a hash table. Something like this `global_sum = local_sum = nums[0]`. Usually the array N is from 0 ~ len\(N\)-1

### 5. Have Two Arrays to Iterate

> If we need to iterate through two arrays, it is possible to have **two pointers \(i, j\)** to traverse/compare both of them.

Merge Two Sorted Arrays

## 常用工具

### 1. Problem Solving Techniques

* **Binary Search: O\(logn\)**
* **Two Pointers: O\(n\)**
* **Sliding Window: O\(n\)**

### 2. 如何遍歷

* Iterate From End to Start 有三種方法 iterate thru index \(1\) `for i in range(len(nums)-1, -1, -1)` start, stop, step/ len\(nums\)-1 is the last element, stop at -1 \(2\) `for i in reversed(range(0, len(nums)-1))` iterate thru element \(3\) `for element in nums[::-1]` list\[start : stop : step\]

#### 

## 易錯點

#### 1. Define Boundaries

如果題需要用index來iterate through every element in the array， 就要特別注意Index Out of Range。

#### 2. Corner Cases

* Empty array: `if not nums or len(nums) == 0`
* Contain Duplicates: `sorted -> remove duplicates` or `nums[i] == nums[i-1]`
* Array with 1 element \(boundary\): `if len(nums) == 1`

#### 3. Defining Cols & Rows



