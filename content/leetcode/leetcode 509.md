---
title: "Leetcode 509"
date: 2022-01-08T20:36:06+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟢 easy 🟢
### 相關主題：Math, Dynamic Programming, Recursion, Memoization

### 題目（英文）
```
The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,

F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n > 1.

Given n, calculate F(n).
```

---

### 測資

```
Example 1:

Input: n = 2
Output: 1
Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.

Example 2:

Input: n = 3
Output: 2
Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.

Example 3:

Input: n = 4
Output: 3
Explanation: F(4) = F(3) + F(2) = 2 + 1 = 3.
```

---

### 題目（中文）

```
費氏數列的值可由前兩項值加總而成

F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n > 1.

給予目標 n，返回其費氏數列 fn
```

---

## 思路

1. 使用動態規劃，紀錄每次當前費氏數列的結果，以避免使用遞迴增加運算速度
---

## 程式碼

```
class Solution {
public:
    int fib(int n) {
        
        int dp[n+1];
        
        for(int i=0;i<n+1;i++){
            if(i==0) dp[i] = 0;
            else if(i==1) dp[i] = 1;
            else dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
};
```

---

## 結果
![](https://i.imgur.com/PAbyTbZ.png)
---

## 結語

以前的遞迴題改成 dp 方法計算