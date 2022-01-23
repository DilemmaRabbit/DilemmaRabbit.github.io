---
title: "Leetcode 53 Maximum Subarray"
date: 2022-01-12T17:15:40+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟢 Easy 🟢

### 相關主題：Array, Divide and Conquer, Dynamic Programming

### 題目（英文）

```
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

A subarray is a contiguous part of an array.
```

---

### 測資

```
Example 1:

Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.

Example 2:

Input: nums = [1]
Output: 1

Example 3:

Input: nums = [5,4,-1,7,8]
Output: 23

 

Constraints:

1 <= nums.length <= 105
-104 <= nums[i] <= 104
```

---

### 題目（中文）

```
給予一個整數陣列，找到一個連續的子陣列，回傳其最大的總和
```

---

## 思路

1. Kadane 算法

- 遍歷 list 內中所有元素

- 每次有兩個選擇
  1. 繼續加總 cur + num[i] > num[i]
  2. 從頭開始 num[i] > cur + num[i]

- 比較並且更新最大值
---

## 程式碼

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        
        int max_cur = 0;
        int max_res = INT_MIN;
        
        for(int i=0;i<nums.size();i++){
            max_cur = max(nums[i], max_cur + nums[i]);
            max_res = max(max_cur, max_res);
        }
        
        return max_res;
    }
};
```

---

## 結果

![](https://i.imgur.com/Y48w7Fq.png)

---

## 結語

思路是將狀況分成重新開始以及繼續，到每個新的節點便比較過去的加總是否會比重新開始高