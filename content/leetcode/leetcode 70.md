---
title: "Leetcode 70"
date: 2022-01-09T13:04:51+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟢 easy 🟢

### 相關主題：Math, Dynamic Programming, Memoization

### 題目（英文）

```
You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
```

---

### 測資

```
Example 1:

Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

Example 2:

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

Constraints:
1 <= n <= 45
```

---

### 題目（中文）

```
你正在爬一個階梯，階梯共有 n 階

你每次可以向上爬 1 或 2 階，請問你有多少種爬到頂端的可能。
```

---

## 思路

1. 起始點位於 0 的位置
   1. ex. 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> ... -> n

2. 對最後一階 n 來說，就只有下列兩種走法
   1. 在 n-1 階時 +1 
   2. 在 n-2 階時 +2 

3. 所以到達第 n 階的所有方法，就是 n-1 階的方法加上 n-2 階的方法加總

4. 回推到最後
   1. 當 n = 1 時，只有 1 種走法
   2. 當 n = 2 時，有 2 種走法
   3. 由此可推出所有的 n 的可能

---

## 程式碼

```
class Solution {
public:
    int climbStairs(int n) {
        
        vector<int> dp{1,2};
        for(int i=2;i<n;i++){
            dp.push_back(dp[i-1] + dp[i-2]); // 紀錄新的一階的可能性
        }
        
        if(n==1){
            return 1; // 避免 n = 1 時判斷錯誤
        }
        
        return dp.back();
    }
};
```

---

## 結果

![](https://i.imgur.com/pZfOOkG.png)
---

## 結語

基本上就是費氏數列的變種，畫圖稍微從尾巴回推可能性就能想到解法了