---
title: "Leetcode 746 Min Cost Climbing Stairs"
date: 2022-01-09T13:05:08+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟢 Easy 🟢

### 相關主題：Array, Dynamic Programming

### 題目（英文）

```
You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index 0, or the step with index 1.

Return the minimum cost to reach the top of the floor.
```

---

### 測資

```
Input: cost = [10,15,20]
Output: 15
Explanation: You will start at index 1.
- Pay 15 and climb two steps to reach the top.
The total cost is 15.

Input: cost = [1,100,1,1,1,100,1,1,100,1]
Output: 6
Explanation: You will start at index 0.
- Pay 1 and climb two steps to reach index 2.
- Pay 1 and climb two steps to reach index 4.
- Pay 1 and climb two steps to reach index 6.
- Pay 1 and climb one step to reach index 7.
- Pay 1 and climb two steps to reach index 9.
- Pay 1 and climb one step to reach the top.
The total cost is 6.

Constraints:

2 <= cost.length <= 1000
0 <= cost[i] <= 999
```

---

### 題目（中文）

```
你被給予一個階梯的費用表 cost，cost[i] 代表當前所在的階梯想要移動需要支付的代價，一旦支付，可以選擇向上爬 1 階或 2 階

你可以從 index 0 或 1 的位置開始

回傳最低爬到頂端的成本
```

---

## 思路

1. 與 leetcode 70 題的爬樓梯稍微不同，越前面的階層越需要從後面的 cost 加總而來

2. 對於爬到頂端，有兩種情況
   1. 在第 n 階支付費用向上 1 階
   2. 在地 n-1 階支付費用向上 2 階

3. 由此可知， n-2 階的成本就是除了自己本身的成本，在加上 n 或 n-1 階的成本（取最小）

4. 最後在判斷開頭的 index 0 與 1 那一個成本較小
---

## 程式碼

```c++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        
        int tmp;
        int csize = cost.size();
        int prev = cost[csize-1]; // n-1 的 cost
        int pprev = cost[csize-2]; // n-2 的 cost
        
        for(int i=csize-3;i>=0;i--){
            tmp = pprev; // 暫存 n-1 的 cost
            pprev = cost[i] + min(prev,pprev);
            prev = tmp;
        }
        
        // 最後會變成 prev = index 1, pprev = index 0 的情況

        return min(prev,pprev);
    }
};
```

---

## 結果

![](https://i.imgur.com/S0noM03.png)
---

## 結語

算是爬樓梯的變體，從由頭開始計算 dp 改成由尾巴回推到最後的結果，提供了另外一個 dp 思考方向