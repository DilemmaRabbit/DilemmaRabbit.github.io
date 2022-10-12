---
title: "718. Maximum Length of Repeated Subarray"
date: 2022-09-20T12:02:49+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]
---

**難度：🟠 Normal 🟠**

**關鍵字：Array, Dynamic Programing**

<!--more-->
---

## 問題描述

給與兩個 array，返回最長公共子陣列的最大值

---

## 思路

1. 每次比對到相同的元素，便去尋找上一個比對的結果並 + 1
```c++
//正序
dp[i][j] = dp[i-1][j-1] + 1
//倒序
dp[i][j] = dp[i-1][j-1] + 1
```  
2. 需要注意處理邊界的狀況
3. 每次更新後比較最大值

BigO:O(n*m)

---

## 程式碼

```c++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        
        int res = INT_MIN;
        int dp[nums1.size()][nums2.size()];
        memset(dp, 0, sizeof(dp));
        
        // 倒序 dp
        // for(int i = nums1.size()-1; i > -1; i--){
        //     for(int j = nums2.size()-1; j > -1; j--){
        //         if(nums1[i] == nums2[j]){
        //             if(i == nums1.size()-1 || j == nums2.size()-1) dp[i][j] = 1;
        //             else dp[i][j] = dp[i+1][j+1] + 1;
        //         }
        //         res = max(res, dp[i][j]);
        //     }
        // }
        
        
        // 正序 dp
        for(int i = 0; i < nums1.size(); i++){
            for(int j = 0; j < nums2.size(); j++){
                if(nums1[i] == nums2[j]){
                    if(i == 0 || j == 0) dp[i][j] = 1;
                    else dp[i][j] = 1 + dp[i-1][j-1];
                }
                res = max(res, dp[i][j]);
            }
        }
        return res;
    }
};
```
