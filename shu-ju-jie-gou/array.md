# Array

基本上是考察對於list, array這種類型的數據結構掌握度。

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

* **Binary Search: O\(logn\)**
* **Two Pointers: O\(n\)**
* **Sliding Window: O\(n\)**

有的時候可以使用`[::-1]`來iterate from end to start。

## 易錯點

#### 1. Define Boundaries

如果題需要用index來iterate through every element in the array， 就要特別注意Index Out of Range。

#### 2. Corner Cases

* Empty array: `if not nums or len(nums) == 0`
* Contain Duplicates: `sorted -> remove duplicates` or `nums[i] == nums[i-1]`
* Array with 1 element \(boundary\): `len(nums) == 1`



