---
title: "Leetcode 1010 Pairs of Songs With Total Durations Divisible by 60"
date: 2022-01-03T16:26:29+08:00
draft: false
Tags: ["LeetCode"]
---

## 問題描述

### 分類：🟠 Medium 🟠
### 相關主題：Counting, Hash_table, Array

---

### 題目（英文）

```
You are given a list of songs where the ith song has a duration of time[i] seconds.

Return the number of pairs of songs for which their total duration in seconds is divisible by 60. Formally, we want the number of indices i, j such that i < j with (time[i] + time[j]) % 60 == 0.
```

---

### 測資

```
Example 1:

Input: time = [30,20,150,100,40]
Output: 3
Explanation: Three pairs have a total duration divisible by 60:
(time[0] = 30, time[2] = 150): total duration 180
(time[1] = 20, time[3] = 100): total duration 120
(time[1] = 20, time[4] = 40): total duration 60

Example 2:

Input: time = [60,60,60]
Output: 3
Explanation: All three pairs have a total duration of 120, which is divisible by 60.

```

---

### 題目（中文）

```
你有一份關於所有音樂的演奏時長的清單

返回清單中任意兩首音樂時長加起來可以被 60 秒整除的數量
```

---

## 思路

1. 由於目標是找到組合加起來為 60 秒的組合，對於超出 60 秒的情況，只要取餘數就可以減少 hash_table 的空間使用
2. 只需要計算 0 ~ 30 秒數量(避免重複)
3. 由於 0 ~ 30 會是取自身的數量做計算，需要跟1~29秒做區隔

---

## 程式碼

```
class Solution {
public:
    int numPairsDivisibleBy60(vector<int>& time) {
        
        int i, cnt = 0;
        map<int, int> table;
        
        for(i = 0; i < time.size(); i++){
            table[time[i]%60] += 1;
        }
        
        for(i=1;i<30;i++){
            if(table[i]!=0){
                if(table[60-i] != 0){
                    cnt += (table[i] * table[60-i]);
                }
            }
        }
        
        if(table[0] >= 2){
            cnt += ((table[0]*(table[0]-1))/2);
        }
        
        
        if(table[30] >= 2){
            cnt += ((table[30]*(table[30]-1))/2);
        }
        
        return cnt;
    }
};
```

---

## 結果

![](https://i.imgur.com/BEgz4K5.png)

---

## 結語
聽朋友說可以用加法的方式來取代原本的乘法，將每一個 1 ~ 29 秒的數量用加的方式計算，可以減少運算時間，改天再試試看～