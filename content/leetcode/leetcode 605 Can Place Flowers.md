---
title: "Leetcode 605 Can Place Flowers"
date: 2022-01-18T22:30:06+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟢 Easy 🟢
### 相關主題：Array, Greedy

### 題目（英文）

```
You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in adjacent plots.

Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule.
```

---

### 測資

```
Example 1:

Input: flowerbed = [1,0,0,0,1], n = 1
Output: true
Example 2:

Input: flowerbed = [1,0,0,0,1], n = 2
Output: false
 

Constraints:

1 <= flowerbed.length <= 2 * 104
flowerbed[i] is 0 or 1.
There are no two adjacent flowers in flowerbed.
0 <= n <= flowerbed.length
```

---

### 題目（中文）

```
給予一片長條地、已經種植部分花的花田，然而，花並不能相鄰著種植

給予一個已經種植了部分花的花田，已種植的部分用 1 表示，未種植則用 0 表示，並且給予一個值 n，代表此花田中最多還能在種的花數量，回傳 true 代表能夠種下 n 朵花，反之則回傳 false
```

---

## 思路

1. 紀錄每個連續的 0 的數量，直接透過數學式產生預計會種的花朵數量

2. 考慮邊界為 0 的狀況
    1. 直接只針對邊界情況做判斷
    2. 將 vector 頭尾補 0 後維持原本計算方法 
---

## 程式碼

```
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        
        int cnt = 0;
        
        flowerbed.insert(flowerbed.begin(),0);
        flowerbed.push_back(0);
        
        for(int i=1;i<flowerbed.size()-1;i++){
            if(flowerbed[i] == 0){
                cout << i;
                if(flowerbed[i-1]==0 && flowerbed[i+1]==0){
                    cnt++;
                    i += 1;
                }
            }
        }
        
        cout << cnt;
        
        if(cnt < n){
            return false;
        }
        
        return true;
    }
};
```

---

## 結果

![](https://i.imgur.com/M4fx1yD.jpg)

---
