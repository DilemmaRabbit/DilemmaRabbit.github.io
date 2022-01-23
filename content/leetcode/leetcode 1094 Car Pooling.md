---
title: "Leetcode 1094 Car Pooling"
date: 2022-01-06T15:03:20+08:00
draft: false
Tags: ["LeetCode"]
---


## 問題描述

### 分類：🟠 Medium 🟠
### 相關主題：Array,Sorting,Heap(Piority Queue),Simulation,Prefix Sum

### 題目（英文）
```
There is a car with capacity empty seats. The vehicle only drives east (i.e., it cannot turn around and drive west).

You are given the integer capacity and an array trips where trip[i] = [numPassengersi, fromi, toi] indicates that the ith trip has numPassengersi passengers and the locations to pick them up and drop them off are fromi and toi respectively. The locations are given as the number of kilometers due east from the car's initial location.

Return true if it is possible to pick up and drop off all passengers for all the given trips, or false otherwise.
```

---

### 測資

```
Example 1:

Input: trips = [[2,1,5],[3,3,7]], capacity = 4
Output: false

Example 2:

Input: trips = [[2,1,5],[3,3,7]], capacity = 5
Output: true
```

---

### 題目（中文）

```
有一輛有座位限制的車，並且這台車只能從西向東行駛( 0 -> N )

你有一份清單 trip，記載了數筆(numPassengersi, fromi, toi)的資料，numPassengers 代表了乘客數量，from 代表起始地點，to 代表目標地點

給定車子的負載 capacity，詢問是否可以將所有乘客載至目的地回傳 true 與 false
```

---

## 思路

1. 由於需要計算每一站上下車的人數(起點與終點)，所以必須針對 trip 內有的資料進行排序
2. 儲存每站需要的上下車人數
3. 當離開某一站時，比較當前的負載是否超重
4. 只要計算當站相對增加或減少的人數直接做加減

---

## 程式碼

基於剛剛1 ~ 3思路

```c++
class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        
        int i;
        int current=0;
        int max=INT_MIN; # 最後一站
        map<int, int[2]> table; # 紀錄每一站的上下車人數資訊
        
        for(i = 0; i < trips.size(); i++){
            table[trips[i][1]][0] += trips[i][0];
            table[trips[i][2]][1] += trips[i][0];
            if(trips[i][2] > max){
                max = trips[i][2]; # 取得最後一站的資料做遍歷用
            }
        }
        
        for(i = 0; i <= max; i++){
            current += table[i][0]; # 上車
            current -= table[i][1]; # 下車
            if(current > capacity){ 
                return false; # 檢查是否超重
            }
        }      
        return true;
    }
};
```

稍微改進了用陣列來儲存與合併負載資訊，4 思路

```
class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        
        int i,cur=0;
        int max=INT_MIN;
            
        for(i = 0; i < trips.size(); i++){
            if(max < trips[i][2]){
                max = trips[i][2]; # 計算出最後一站( 需要的array大小 )
            }
        }
        
        int t[max+1]; # 由於包含 0，需要在增加 1 大小
        memset(t,0,sizeof(int)*max); # 陣列初始化
        
        for(i = 0; i < trips.size(); i++){
            # 取得此站總共需要的上下車人數總和
            t[trips[i][1]] += trips[i][0]; 
            t[trips[i][2]] -= trips[i][0];
        }
        
        for(i = 0; i < max; i++){
            cur += t[i];
            if(cur > capacity){
                return false; # 檢查是否超重
            }
        }
        return true;
    }
};
```

---

## 結果

第一個作法

![](https://i.imgur.com/icOlS6d.png)

第二個作法

![](https://i.imgur.com/0pUEMMw.png)
---

## 結語

昨天的 dp 題完全寫不出來 QQ (改天在寫)，今天的題目概念算是簡單的類型，慢慢計算上下車的人數就好，後來發現直接計算人數的相對增減更省空間，一開始用 hash table 是為了方便計算 max 的值，由於如果需要紀錄每一站的上下車人數，必須要事先知道最後一站的編號，hash table 在建立的過程中就可以取得，但是換成用 array 的方式就必須先跑一層 for 來確定 max，但是以結果來說還是可以明顯發現 hash table 所佔用的資源量比較多，且跑起來的平均表現也沒有比用一般 array 好。