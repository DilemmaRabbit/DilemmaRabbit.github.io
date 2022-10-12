---
title: "1578. Minimum Time to Make Rope Colorful"
date: 2022-10-03T09:45:24+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]
---

**難度：🟠 Normal 🟠**

**關鍵字：Array**

<!--more-->
---

## 問題描述

- 給予一串氣球，透過移除部分氣球，達到不連續重複的排列
- 給予一個字串 colors，重複的字母代表同一個顏色
- 給予一個 vector neededTime，代表對應 clolrs 的字元所需要的時間
- 讓 colors 形成一個沒有連續重複字元的字串，並且計算最小所需要花費的時間

---

## 思路

- 字串中尋找重複的字元並移除
  - 遍歷字串
    - 新的字元：記錄目前的 index 及字元
    - 重複的字元：與 index 的時間進行比較，並加總時間，並且根據大小將 index 做更新

---

## 程式碼

```c++
class Solution {
public:
    int minCost(string colors, vector<int>& neededTime) {

        int res = 0;
        // char cur_dup = '\0';
        // int cur_min;
        // int size = colors.size();

        for(int i = 0; i < colors.size()-1 ;i++){
            if(colors[i] == colors[i+1]){
                res += neededTime[i] > neededTime[i+1] ? neededTime[i+1] : neededTime[i];
                // 將目前可能被替換的 neededTime 移至 i+1，進行下次 i+2 時的比較 
                neededTime[i+1] = neededTime[i] < neededTime[i+1] ? neededTime[i+1] : neededTime[i];
            }
        }

        // for(int i = 0; i < size; i++){
        //     if(cur_dup != colors[i]){
        //         cur_dup = colors[i];
        //         cur_min = neededTime[i]; 
        //     }
        //     else{
        //         res += cur_min > neededTime[i] ? neededTime[i] : cur_min;
        //         cur_min = cur_min < neededTime[i] ? neededTime[i] : cur_min;
        //     }
        // }

        return res;
    }
};
```
