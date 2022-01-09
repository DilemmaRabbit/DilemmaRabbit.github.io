---
title: "Leetcode 1137 N-th Tribonacci Number"
date: 2022-01-08T20:37:01+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟢 Easy 🟢
### 相關主題：Math, Dynamic Programming, Memoization

### 題目（英文）

```
The Tribonacci sequence Tn is defined as follows: 

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

Given n, return the value of Tn.
```

---

### 測資

```
Example 1:

Input: n = 4
Output: 4
Explanation:
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4

Example 2:

Input: n = 25
Output: 1389537
```

---

### 題目（中文）

```
Tribonacci 可以表示為以下式子

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

給予一個 n，回傳 tn
```

---

## 思路

1. 由 leetcode 509 改變而來，多判斷一個 t2 = 1
---

## 程式碼

```
class Solution {
public:
    int tribonacci(int n) {
    
        int dp[n+1];
        
        for(int i=0;i<n+1;i++){
            if(i==0) dp[i] = 0;
            else if(i==1) dp[i] = 1;
            else if(i==2) dp[i] = 1;
            else dp[i] = dp[i-1] + dp[i-2] + dp[i-3];
        }
        
        return dp[n];
    }
};
```

---

## 結果
![](https://i.imgur.com/8MAzYj8.png)
---

## 結語

以前的遞迴題改成 dp 方法計算