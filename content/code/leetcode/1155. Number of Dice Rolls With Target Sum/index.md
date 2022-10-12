---
title: "1155. Number of Dice Rolls With Target Sum"
date: 2022-10-02T12:59:55+08:00
draft: false
categories: ["Leetcode"]
Tags: ["LeetCode"]
---

**難度：🟠 Normal 🟠**

**關鍵字：Dynamic Programing, Recursive**

<!--more-->
---

## 問題描述

- 給予三個數字，n、k、target，n 代表骰子數量，k 代表是一個 k 面的骰子，target 為目標的總和
- 返回總共可以產生 target 的所有排列數量，答案需要小於 10^9+7
- ex. n = 2, k = 6, target = 7，可產生的結果有 1+6, 2+5, 3+4, 4+3, 5+2, 6+1 6種

---

## 思路

- 遞迴暴力解
  - 透過減少骰子數量，可以推導出最後剩下一顆骰子時的狀況
  - 減少一顆骰子，target 可以減少 1 ~ k，再由此 n - 1、target - 1 ~ k 的狀況，推導下一次遞迴
    - 需要考慮當 dice 數量可形成的 target 最大與最小值
    - 當骰子數量為 1 時，無法再進行分割，因此回傳排列數量 1
  - 問題：每次在不同的取值下，有可能取到相同的 target, dice 數量，但是卻必須重新計算所有遞迴的結果
- DP 優化
  - 建立一個 vector，用來記錄可能的結果數量，需要 dice 列、 target 行(變動)，以減少遞迴次數
    - 每列代表目前有 n 顆骰子
    - 每行代表在 n 顆骰子的情況下，有 1 ~ n * faces 可能的結果(其實也不會用到 1 ~ n-1 的位置)
  - 如果查找的結果為 0 ，則進行遞迴找值，否則直接取值

---

## 程式碼

```c++
class Solution {
public:

    // dp[dices][target] = possible result;
    vector<vector<int>> dp;
    
    long long numRolls(int dices, int faces, int target){
        long long res = 0;
        
        // If result is not recorded in vector
        if(dp[dices][target] == 0){ 
            if(dices==1){
                // Set 1 because target can't be split by two more dices
                dp[dices][target] = 1; 
            }
            else{
                // iterate all possible value for current dice
                for(int i = 1; i <= faces; i++){ 
                    // If remaining value(target - i) in range((dices-1)*faces)
                    if(target - i <= (dices-1)*faces && dices-1 <= target - i ){ 
                        // Enter next recursive
                        res += numRolls(dices-1, faces, target-i);
                    }
                }
                // mod 1000000007 for the vector
                dp[dices][target] = res%1000000007;
            }
        }

        // return current answer
        return dp[dices][target];
    }

    int numRollsToTarget(int n, int k, int target) {
        
        if(n*k < target) return 0; // Targer greater than maximun possible value
        
        for(int i = 0; i <= n; i++){
            dp.push_back(vector<int>(i*k+1)); // Create vector like {face*0, face*1, face*2.......}
        }

        return numRolls(n, k, target); // Return the answer
    }
};
```
