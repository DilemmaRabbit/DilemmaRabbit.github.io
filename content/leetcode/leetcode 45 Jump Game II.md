---
title: "Leetcode 45 Jump Game II"
date: 2022-01-11T13:34:33+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟠 Medium 🟠

### 相關主題：Array, Dynamic Programin, Greedy

### 題目（英文）

```
Given an array of non-negative integers nums, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.
```

---

### 測資

```
Example 1:

Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

Example 2:

Input: nums = [2,3,0,1,4]
Output: 2

 

Constraints:

1 <= nums.length <= 104
0 <= nums[i] <= 1000


```

---

### 題目（中文）

```
你被給予一串非負數的數列，你從 first index 開始，陣列值代表你一次最多可以移動到其他 index 的距離

你的目標是計算出最少可以到達終點的步數

假設一定存在到終點路徑
```

---

## 思路

1. 由最後的位置推回去
   
2. 假設最後的距離為 0
   
3. 當前的 index 的最小移動步數一定是可移動範圍內的最小值 +1

4. 基於上述方法，最後直接取 dp[0] 作為最後的答案

---

## 程式碼

```
class Solution {
public:
    int jump(vector<int>& nums) {
         
        int dp[nums.size()];
        dp[nums.size()-1] = 0;
    
        for(int i=nums.size()-2;i>=0;i--){
            int min_val = nums.size();
            for(int j=i+1;j<=i+nums[i];j++){
                if(j >= nums.size()){
                    break;
                }
                min_val = min(min_val, dp[j]);
            }
            dp[i] = min_val+1;        
        }
        return dp[0];
    }
};
```

---

## 結果

!()[https://i.imgur.com/4reXLIt.png]
---

## 結語

跑出來的速度不盡理想，由於每個值還是需要計算範圍內的最小值，會花不少時間。相較於網路上用 BFS 來進行解題的方法。