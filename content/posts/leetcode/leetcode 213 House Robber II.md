---
title: "Leetcode 213 House Robber II"
date: 2022-01-10T14:03:58+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟠 Medium 🟠

### 相關主題：Array, Dynamic Programming

### 題目（英文）

```
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.
```

---

### 測資

```
Example 1:

Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.

Example 2:

Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.

Example 3:

Input: nums = [1,2,3]
Output: 3

Constraints:
1 <= nums.length <= 100
0 <= nums[i] <= 1000
```

---

### 題目（中文）

```
你是一個強盜，準備去偷一條街上的房子，但每間房子都有防盜措施，當偵測到連續兩間房子被偷竊時，警鈴將會響起。

然而房子呈現一個環狀排列，意味著當你偷了第 1 間房子後，如果又偷了第 n 間房子，警鈴將會響起。

給你一個一條街上房子的金額，在不觸動警鈴的情況下，找出最大的可獲得金額。
```

---

## 思路

1. 基本上與 [House Robber](https://dilemmarabbit.github.io/leetcode/leetcode-198-house-robber/) 相同
   1. 如果不偷第一間(代表最後一間可以偷) -> 第 2 ～ n 間的 House Robber
   2. 如果要偷第一間(代表最後一間不可以偷) -> 第 1 ~ n -1 間的 House Robber

2. 分開寫兩種情況

---

## 程式碼

```
class Solution {
public:
    int rob(vector<int>& nums) {
        
        int res = 0;
        int tmp, notake, take;
        
        if(nums.size() == 1){
            return nums[0];
        }
        
        for(int i = 0; i < nums.size() - 1; i++){
            if(i == 0){
                notake = 0;
                take = nums[i];
            }
            else{
                tmp = notake;
                notake = max(take, notake);
                take = tmp + nums[i];
            }
            
            if(res < max(take, notake)){
                res = max(take, notake);
            }
        }
        
        for(int i = 1; i < nums.size(); i++){
            if(i == 1){
                notake = 0;
                take = nums[i];
            }
            else{
                tmp = notake;
                notake = max(take, notake);
                take = tmp + nums[i];
            }
            if(res < max(take, notake)){
                res = max(take, notake);
            }
        }
        
        
        return res;
    }
};
```

---

## 結果

![](https://i.imgur.com/DSuuXSN.png)
---

## 結語

算是 [House Robber](https://dilemmarabbit.github.io/leetcode/leetcode-198-house-robber/) 的變種題型，但其實就是把兩種情況分開討論而已，考慮合併一起計算，應該能縮短實際上的計算時間(儘管已經是 0 ms)

1. 無論是否要偷第一間，在考慮的當下時，就不能考慮第 n 間的情況了
   1. 偷第一間的話第 n 間偷的情況不存在，因此只要考慮 n - 1 間時的最大值就可以了
   2. 不偷的話，就是第二間開始偷到第 n 間的狀況了