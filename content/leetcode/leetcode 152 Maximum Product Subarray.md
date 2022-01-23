---
title: "Leetcode 152 Maximum Product Subarray"
date: 2022-01-03T23:12:51+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟠 Medium 🟠

### 相關主題：Array, Dynamic Programing

### 題目（英文）

```
Given an integer array nums, find a contiguous non-empty subarray within the array that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a 32-bit integer.

A subarray is a contiguous subsequence of the array.
```

---

### 測資

```
Example 1:

Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
Example 2:

Input: nums = [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
 

Constraints:

1 <= nums.length <= 2 * 104
-10 <= nums[i] <= 10
The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.
```

---

### 題目（中文）

```
給予一個整數的陣列，找到一個連續的非空子陣列，並且此子陣列具有最大的乘積

測資可以保證乘積在 32 bit 以內
```

---

## 思路

1. 以順向用 dp 的方式來乘積出可能的最大值

2. 以逆向用 dp 的方式來乘積出可能的最大值

3. 兩者之中會決定要保留的是最左邊或最右邊的負數值@

---

## 程式碼

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        
        int max_val = INT_MIN;
        int tmp = 1;
        int first=0;
        
        for(int i = 0 ; i < nums.size() ; i++){
            tmp *= nums[i];
            max_val = max(tmp,max_val);
            if(nums[i] == 0){
                tmp = 1;
            }
        }
        
        tmp = 1;
        
        for(int i = nums.size()-1 ; i >= 0 ; i--){
            tmp *= nums[i];
            max_val = max(tmp, max_val);
            if(nums[i] == 0){
                tmp = 1;
            }
        }
        
        return max_val;
    }
};
```

---

## 結果

![](https://i.imgur.com/d3QSgNE.png)