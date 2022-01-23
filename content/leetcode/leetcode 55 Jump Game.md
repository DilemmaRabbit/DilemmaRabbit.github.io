---
title: "Leetcode 55 Jump Game"
date: 2022-01-11T13:35:05+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟠 Medium 🟠

### 相關主題：Array, Dynamic Programin, Greedy

### 題目（英文）

```
You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.
```

---

### 測資

```
Example 1:

Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

Example 2:

Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

 

Constraints:

1 <= nums.length <= 104
0 <= nums[i] <= 105
```

---

### 題目（中文）

```
你被給予一串數列，你從 first index 開始，陣列值代表你一次最多可以移動到其他 index 的距離

如果可以到達最後的 index，回傳 true 
```

---

## 思路

1. 由於不會有負數，代表需要特別注意的只有 0 的狀況才有可能讓你到不了最後的 index

2. 用一個值紀錄目前可以到達到最遠的 max_index，每次到新的 index 時都會去重新計算 max_index

3. 有另外兩種情況需要注意
   1. 遇到 0：判斷目前 max_index 是否大於 0 的 index，代表能不能跨過這個 0
   2. max_index 直接到終點：直接回傳 true

4. 跑到最後，代表 max_index 大於終點的 index，回傳 true
---

## 程式碼

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        
        int max_index = 0;
        
        if(nums.size() == 1){
            return true;
        }
        
        for(int i = 0; i < nums.size(); i++){
            max_index = max(max_index, i + nums[i]);
            if(max_index == nums.size()-1){
                return true;
            }
            if(nums[i] == 0){
                if(max_index <= i){
                    return false;
                }
            }
        }
        
        return true;
    }
};
```

---

## 結果

![](https://i.imgur.com/8C1jYJV.png)

---

## 結語

唯一的阻擋只有 0，所以只要當碰到 0 時考慮計算如何繞過去就可以了。