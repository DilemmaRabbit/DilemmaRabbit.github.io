---
title: "985. Sum of Even Numbers After Queries"
date: 2022-09-21T11:43:56+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]
---

**難度：🟠 Normal 🟠**

**關鍵字：Dynamic Programing**

<!--more-->

---

## 問題描述

給予兩個陣列 nums / queries，queries[i][0]代表增加的數值，queries[i][1]代表需要增加的 nums index，每次計算完後，只紀錄 nums 中的偶數總和，回傳一個與 nums 大小相同的結果陣列

---

## 思路

需要重複計算的部分為每次修改完 nums 後，重新進行總和的部分，因此可以先將總和計算出來，依據當前回合的結果，判斷需要加或減的部分，直接對總和進行運算，最後再更新新的結果到 nums 上，省去每次遍歷的過程

---

## 程式碼

```c++
class Solution {
public:
    vector<int> sumEvenAfterQueries(vector<int>& nums, vector<vector<int>>& queries) {
        
        vector<int> res;
        int tmp = 0;
        
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] % 2 == 0) tmp += nums[i];
        }
        
        for(int i = 0; i < queries.size(); i++){
            
            if((nums[queries[i][1]] + queries[i][0]) % 2 == 0){ // 如果加總後是偶數
                if(nums[queries[i][1]] % 2 == 0){ 
                    tmp += queries[i][0]; // 偶數的情況下，因為結果已經具有了原數值，只需要加上增減的部分
                }
                else{
                    tmp += nums[queries[i][1]] + queries[i][0]; // 奇數的情況下便加上總和
                }
            }
            else{ // 如果加總後是奇數
                if(nums[queries[i][1]] % 2 == 0){
                    tmp -= nums[queries[i][1]]; // 原本是偶數的狀況下，要減掉自身                    
                }
                // 奇數變奇數，不會更新結果，只需要更新 nums
            }
            
            nums[queries[i][1]] = nums[queries[i][1]] + queries[i][0]; // 更新 nums
            res.push_back(tmp);
        }
        
        return res;
    }
};

class Solution {
public:
    vector<int> sumEvenAfterQueries(vector<int>& nums, vector<vector<int>>& queries) {
        
        vector<int> res;
        int tmp = 0;
        
        // 計算總和
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] % 2 == 0) tmp += nums[i];
        }
        
        for(int i = 0; i < queries.size(); i++){
            
            // 取得目前 query 的 index 與 value
            int index = queries[i][1];
            int val = queries[i][0]; 
            
            if(nums[index] % 2 == 0) tmp -= nums[index]; // 如果是偶數，都先從總和中減掉
            val += nums[index]; // 計算加總後結果
            if(val % 2 == 0) tmp += val; // 如果是偶數再加回去
            
            nums[index] = val; // 更新 nums
            res.push_back(tmp);
        }
        
        return res;
    }
};
```
