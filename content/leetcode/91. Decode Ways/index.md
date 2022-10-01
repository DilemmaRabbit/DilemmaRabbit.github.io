---
title: "91. Decode Ways"
date: 2022-10-01T14:46:09+08:00
draft: false
categories: ["Learning"]
Tags: ["LeetCode"]

resources:
  - name: "featured-image"
    src: "leetcode.png"
---

難度：🟠 Normal 🟠

---

## 問題描述

給予一個包含數字的字串，字串內的字元可以轉換為對應的英文字母(A=1、B=2......Z=26)，一個數字字串可能可以轉為多種不同的結果，返回所有可能的結果數量。
如果無法完全處理整個字串，則回傳 0，代表沒有可能的結果。

---

## 思路

- DFS(Time Limit Exceed)
  - 設定一個 index，表示此 index 前(包含)的字元已經過檢查，可形成結果
  - 每次檢查 s[index+1] , s[index+1:index+2] 的字串結果，如果通過檢查，將 index +1 / +2 的結果進入下一次 DFS
  - 如果到達了字串的末尾，代表成功產生結果
  - **盲點：111可分為 1 11 / 11 1 / 1 1 1 三種結果，但是需要計算 3 次，無法重複使用已知的結果**

- DP
  - 建立一個 vector，代表第 N 個字串前的結果數量 -> dp[size()+1] // dp[0] 不紀錄結果資訊
  - 當加入了一個新的字元，有兩種可能結果
    - 字元本身可以成為新的結果
    - 字元可以與前一個字元產生新的結果
  - **然而需要考慮不符合的情況(沒有產生結果)**
  - 算法類似費式數列
    - 字元本身可以成為新的結果 -> dp[i] += dp[i-1];
    - 字元可以與前一個字元產生新的結果 -> dp[i] += dp[i-2]
  - 最後回傳 dp[size()]
  - **dp[0] 必須是 1**
    - 由於第一個字元會參考到 dp[i-1]，如果判斷式成立的話會是一種結果(也可以直接附值 0)
    - 由於第二個字元會參考到 dp[i-2] 的結果，如果判斷式成立的話會是一種結果

---

## 程式碼

```c++
class Solution {
public:
    // dfs(TLE)
    // int res = 0;
    // string str;

    // void dfs(int index){
    //     if(index == str.size()-1){
    //         res++;
    //         return;
    //     }
        
    //     if(index == str.size()-2){
    //         if(str[index+1] != '0') dfs(index+1);
    //     }
    //     else{
    //         if(str[index+1] != '0') dfs(index+1);
    //         if("09" < str.substr(index+1, 2) && str.substr(index+1, 2) < "27") dfs(index+2);
    //     }
    // }

    // int numDecodings(string s) {
    //     str = s;
    //     dfs(-1);
    //     return res;    
    // }

    int numDecodings(string s){

        vector<int> dp(s.size()+1);
        dp[0] = 1;

        for(int i = 0; i < s.size(); i++){
            // 第一個字元
            if(i == 0){
                if(s[i] == '0') return 0;
                else dp[i+1] = 1;
                // else dp[i+1] = dp[i];
            }
            // 第二個字元
            else{
                if(s[i] != '0') dp[i+1] += dp[i];
                if("09" < s.substr(i-1, 2) && s.substr(i-1, 2) < "27") dp[i+1] += dp[i-1];
            }
        }
        return dp[s.size()];
    }
};
```
