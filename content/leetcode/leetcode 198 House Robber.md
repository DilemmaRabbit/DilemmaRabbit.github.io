---
title: "Leetcode 198 House Robber"
date: 2022-01-10T14:03:44+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟠 Medium 🟠

### 相關主題：Array, Dynamic Programming

### 題目（英文）

```
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.
```

---

### 測資

```
Example 1:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

Example 2:

Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

---

### 題目（中文）

```
你是一個強盜，準備去偷一條街上的房子，但每間房子都有防盜措施，當偵測到連續兩間房子被偷竊時，警鈴將會響起。

給你一個一條街上房子的金額，在不觸動警鈴的情況下，找出最大的可獲得金額。
```

---

## 思路

1. 將每間的情況分成兩種(偷與不偷)
   1. 偷：取上一間沒有偷的情況下的金額加上現在這間的金額
   2. 不偷：取上一間偷與不偷的最大值
   
2. 每間結束後，判斷當前的金額是否為最大值的金額

3. 第一間的狀況
   1. 偷：取第一間的金額
   2. 不偷：0 元

---

## 程式碼

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        
        int max = INT_MIN;
        int tmp;
        int take; // 當前房屋偷取的情況下，所獲得的最大值
        int nottake; // 當前房屋不偷的情況下，所獲得的最大值
        for(int i=0;i<nums.size();i++){
            if(i == 0){
                take = nums[i];
                nottake = 0;
            }
            else{
                tmp = nottake;
                if(take > nottake){
                    nottake = take;
                }
                take = tmp + nums[i];
            }
            
            if(take > max){
                max = take;
            }
        }
        
        return max;
    }
};
```

---

## 結果

![](https://i.imgur.com/BktydiU.png)
---

## 結語

將狀況拆分成兩種，並且將天數縮小至一天～兩天時，就可以開始推估狀況，可以找到規律了。